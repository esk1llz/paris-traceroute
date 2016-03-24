

# Data Storage #

MDA stores its data in the structure “mda\_data\_t”, shown below.

```
// libparistraceroute/algorithms/mda/data.h

typedef struct {
    lattice_t * lattice;
    uintmax_t   last_flow_id;
    address_t * dst_ip;
    pt_loop_t * loop;
    probe_t   * skel;
    bound_t   * bound;
} mda_data_t;
```

There are two basic types of data associated with the MDA. Global data – information pertinent to the current MDA instance as a whole –, and interface-specific data, contained in the lattice. The former is what is immediately presented to you in the above data structure, whereas the latter requires going a level deeper, into the lattice structure.

## Global Data ##

  * _lattice:_ see lattice (below)
  * _last\_flow\_id:_ the MDA generates new flow\_ids as needed. This is done by keeping track of the most recently generated id and then incrementing this id when a new one is needed.
  * _dst\_ip:_ the destination IP address given to MDA to discover in the network.
  * _loop:_ the MDA is an algorithm to be used in a traceroute; however, it is not a standalone program. This pointer makes reference to the “paris-traceroute loop” that represents the heartbeat of the higher level traceroute program. It is from here that the MDA is itself called.
  * _skel:_ basic structure for probes that are to be sent. A probe is made ready to send by filling in certain choice fields, namely `"ttl"` and `"flow_id"`.
  * _bound:_ structure that contains access to the table of stopping points.

## Lattice ##

The lattice is used for storing the network graph as it is discovered. It is represented by a dynamic array of roots, or starting points for the graph. These roots are of type `lattice_elt_t`. In the case of the MDA, we will only be using graphs with a single root – the source interface.

```
// libparistraceroute/lattice.h

typedef struct {
    dynarray_t * next;
    dynarray_t * siblings;
    void       * data;
} lattice_elt_t;
```

This is a basic lattice element, or node in the graph.
  * _next:_ contains all the children, or successors, connected to the current element.
  * _siblings:_ contains all the elements that are at the same depth in the graph from a common ancestor.
  * _data:_ each lattice node also contains some data specific to itself. In the case of the MDA, this data will be of type `mda_interface_t`. Here lies the interface-specific data, where the vast majority of data manipulation will take place (see next section).

```
// libparistraceroute/algorithms/mda/interface.h

typedef struct {
    address_t     * address;
    size_t          sent,
                    received,
                    timeout,
                    num_stars;
    dynarray_t    * ttl_flows;
    uint8_t         ttl_set[MAX_TTLS];
    size_t          num_ttls;
    bool            enumeration_done;
    mda_lb_type_t   type;
} mda_interface_t;
```

  * _address:_ is the IP address of the current interface.
  * _sent, received, timeout, num\_stars:_ Record keeping pertaining to quantities of probes sent and quantities of the various results they incurred.
  * _ttl\_flows:_ is an array of TTL/flow pairs. A TTL/flow pair contains all the information necessary to send a probe to a particular interface. This dynamic array contains all such pairs that have thus far reached this interface.
  * _ttl\_set:_ is the set of TTLs at which this interface can be found. This information could also be found by going through the ttl\_flows array, but it is provided here as well for easier reference. Capped in size by the constant `MAX_TTLS`.
  * _num\_ttls:_ The number of different TTLs at which this interface can be found. This information could also be found by going through the ttl\_flows array, but it is provided here as well for easier reference.
  * _enumeration\_done:_ Indicates that whether or not this interface is done sending probes and thus whether or not it can now be classified.
  * _mda\_lb\_type\_t:_ The type of interface this is classified as.

# Data Manipulation #

## mda\_handler\_init ##

This short function is responsible for initializing an instance of MDA. It initializes a reference to mda\_data\_t through `mda_data_create` (see libparistraceroute/algorithms/mda/data.c), fills out the data structure's fields, and pushes a “dummy” root into the instance's lattice.

## mda\_handler\_reply ##

This function is called in the event that a reply to a probe is detected. It can be broken down into the following phases.

> ### Handling an event ###

The `event_t` structure (an argument given to each of the handlers), contains both the initial probe and the reply in its “data” field. These are each extracted from the event and saved as `probe_t`s, named probe and reply, respectively:
  * From reply we can extract the address of the interface that sent it (that is, the interface that the probe has discovered).
  * From probe we can extract the flow\_id and ttl to which it was sent.

> ### Searching for the discovered interface: ###

With the address extracted above, we can search our lattice and determine whether or not it already contains the discovered interface. Here is our first exposure to the ̀ lattice\_walk`. The  `lattice\_walk` function, as its name connotes, performs a walk through each of the nodes in a given lattice. This can be done either as a depth first search (DFS) or a breadth first search (BFS), although BFS is as of yet unimplemented. `lattice\_walk` is passed a callback function (all defined within `mda.c` and generally simple to understand) and some generic data. The callback function will perform some operation(s) on each interface discovered during the walk, given the generic data. To search for the discovered interface, we perform a `lattice\_walk` with the callback `mda\_search\_interface` and the extracted address as data.

If the interface is not found in the lattice, this means we have never encountered it before. Therefore, MDA creates a `new mda_interface_t` with the given address and adds the current ttl to its list of ttls.

> ### Searching for the initial interface: ###

This is the parent to the discovered interface. As such, it is the interface that is reachable with exactly the same flow\_id as contained by the probe sent to the discovered interface, but at a hop one closer to the source (ttl – 1). Again with a `lattice_walk`, we can search for this node in the graph. We use the callback `mda_search_source`, and the flow\_id and ttl - 1, extracted from probe, as data.

> ### Updating lattice: ###

> If we were unable to find the initial interface above, then nothing is done and the handler terminates. Otherwise, if the discovered interface was found as already existing in the lattice, then we must connect it to the initial (parent) interface if it is not already connected. This is done in `lattice_connect`.

If the discovered interface is new to the lattice then we must add it with `lattice_add_element`. The flow\_id and TTL that reached the discovered interface are pushed onto the interface's `ttl_flow` dynamic array as a TTM/flow pair.

## mda\_handler\_timeout: ##

This has a similar structure to mda\_handler\_reply. The difference is that now, instead of having a reply probe, we only have the initially sent probe (it timed-out without response). Thus we perform the same process of searching for the parent node. Then, a new “star” interface is created and connected to the parent node in the lattice.

# Probe Sending #

Probes are sent by a `lattice_walk` with a special callback function called `mda_process_interface`. This function “processes” each interface in the lattice in 2 steps:
  1. _Classification_ is handled by `mda_classify` and is a straightforward function that marks what type of interface a given interface is.
  1. _Enumeration_ is handled by `mda_enumerate` and is slightly more complicated. This is where the probes are actually sent.

## mda\_enumerate ##

The enumeration of an interface proceeds as follows. First, the number of probes left to send is calculated. This is a simple calculation made with the stopping points from mda\_data\_t's bound structure as well as the number of packets already sent at that interface (`mda_interface_t->sent`). The number of probes left to send is the stopping point minus the number of probes already sent.

Next, if the interface has multiple siblings, we must check to see if it has enough available flow\_ids to satisfy the number of probes it must send. If it does not, the MDA increments the last\_flow\_id and sends out a probe with this id to one of the ttls contained in its set of ttls. This is done as many times as there are missing flow\_ids, varying evenly over all the TTLs contained in the `ttl_set`. If an interface has no siblings, then any probe sent to its TTL will theoretically succeed in finding it. Thus we do not need to test out any additional flow\_ids, and instead have an infinite number at our disposal. It is important to note that in the case of extra flow\_ids being generated, it is possible for the probes that carry them to arrive at a different interface from the one that is intended. If the interface we are currently enumerating has siblings, a probe sent to its ttl could reach one of its siblings instead. This represents performing additional discovery for the parent interface, and thus the extra probes sent in this part of the enumeration of an interface are in addition to the numbers determined by MDA's stopping points.

The last step in enumeration is sending off probes for further discovery of the lattice. This is done by taking each available flow\_id and sending a probe containing that id to the ttl one greater than the one associated with the id. This is done as many times as there are probes to send.

# Action flow of the MDA #

With the above components explained, you should be able to see, at least generally, to what end each of the functions within `mda.c` progress. Knowing that, the flow of actions in the MDA is fairly straightforward.

The MDA is “woken up” every time some event pertinent to it occurs in the outside loop (the pt\_loop, which handles all events – user, network, etc - in a given run of a program). “Waking up” entails passing an event to the MDA's main event handler, mda\_handler. At the start of the program, an “init event” is sent to the MDA. The mda\_handler sends this off to `mda_handler_init`, which, as described above, initializes key data structures in the MDA. While the program runs, if a “reply event” or a “timeout event” is received, the MDA is likewise woken up and mda\_handler sends off the event to the appropriate locations, either mda\_handler\_reply or mda\_handler\_timeout. At the end of any event (init, reply, timeout) `mda_handler` ALWAYS runs a `lattice_walk` with the `mda_process_interface` callback. Thus, at the end of any event in the mda, new probes will be sent out when required, from all over the lattice. This is what keeps the program running until termination. Once `mda_handler` terminates, control is returned to the pt\_loop until a new event pertinent to MDA occurs. Because no probes are sent beyond the destination interface, once this interface is discovered, MDA events will slowly dwindle in number. Eventually, all interfaces in the discovered graph will have fulfilled their probe-sending requirements, and once the last probe either replies or timeouts, MDA will no longer be woken up and the `paris-traceroute` program can terminate. Upon termination of paris-traceroute, the lattice is printed through lattice\_dump.

Notice that MDA has only two tasks: data manipulation in response to an event, and probe-sending based off of the state of the data. MDA itself is not responsible for waiting for probe replies or any other network-related tasks. The algorithm merely reviews the lattice of discovered interfaces and sends as many probes as needed. When an event comes back to the algorithm as a result of any one of these probes, it updates its own data accordingly and then enumerates all discovered interfaces once more.