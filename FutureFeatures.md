List of features to be developed, ordered by priority:



# Metafields #

A metafield in an abstract field made of one ore more fields. For instance we would like to define a "flow\_id" metafield made (in IPv4) of the well-known (src\_ip, dst\_ip, src\_port, dst\_port, protocol\_id) tuple.

This could be also used to abstract in an algorithm (depending on the probe structure) where we write the tag (identifier) of a probe packet (see next section).

_Tasks:_
  * Finalize bits module
  * Define some metafields (flow\_id for IPv4/{UDP, TCP} etc.)
  * Tests

# Probe tag customization #

Each algorithm should be able to define where is written its probe tag (i.e. the part of the packet where we write an identifier). For instance, legacy traceroute encode its tag in the source port, whereas paris-traceroute encodes its tag
  * in the UDP (resp. TCP) checksum for IPv?/UDP (resp. IPv?/TCP) measurements
  * in the sequence number for IPv?/ICMPv? measurements

The library should allow an algorithm to define where the tag is encoded (for now, the probe-tagging is hardcoded in the network layer and the tag is stored in the last checksum field).

The network layer could store a tree where each node is related to a protocol and a branch of this tree corresponds to a protocol pattern (for instance IPv4/ICMP/UDP/ICMP). Therefore, we could store for each pattern a set of tags. This pattern could be announced to the algorithm by exposing a "probe\_tag" metafield (see previous section).

_Tasks:_
  * Finalize metafields.
  * Enhance network layer.
  * Test with several algorithm instances using the same or different tag encoding.

# Enhance socketpool/sniffer #

In the current implementation, our socketpool the entire packet, in our case to tweak the UDP checksum. According to the place where we encode the tag (for instance src\_port) we do not necessarily require raw socket. If so, the socketpool should not create raw socket.

The same problem also arises for packet sniffing.

Tasks:
  * Implement bitfields to guess which parts of the packet are set by the user/running algorithm.
  * Create appropriate sockets according to the packet we want to send/recv.

# Fix memory leaks #

Some instances are not properly freed in the current implementation of libparistraceroute, especially if an unexpected error or if the program is stopped abruptly. It especially concerns probe\_t and packet\_t instance which are duplicated as less as possible and pass layer by layer.

We might require to count how many times such an object is referenced and adapt the corresponding free function.

# Enhance event management #

The event management design is not yet stable in libparistraceroute. The prototype of our handlers and the way we create events may evolve and should be as simple as possible.

# Suggestions #

  * allowing to share information between generators,
  * what if a reply matches several probe packets,
  * detect "rare" cases,
  * how to split a packet in a several way (fragmentation) (falungong), send this fragment n times etc.