# Paris Traceroute Ideas Page #

<table cellpadding='10' width='825' border='1'><tr><td>
<b>March 2012 update:</b> Paris Traceroute is proud to be participating in the <a href='http://www.google-melange.com/gsoc/homepage/google/gsoc2012'>Google Summer of Code 2012</a> under the "umbrella" of <a href='http://measurementlab.net/'>Measurement Lab</a>, or M-Lab. We welcome potential student developers to discuss with us on our IRC channel, #paris-traceroute at irc.freenode.net, or via our developer mailing list in the <a href='https://groups.google.com/forum/?fromgroups#!forum/paris-traceroute'>Paris Traceroute Google Group</a> (<a href='mailto:paris-traceroute@googlegroups.com'>paris-traceroute@googlegroups.com</a>).<br>
</td></tr></table>

The Paris Traceroute library, libparistraceroute, provides a convenient framework on which to build packet probes and measurement algorithms. Experimenters can easily implement active measurement tools based on ping or traceroute style network probes using the library. If you would like to have an impact on the world of network measurements, please consider joining us to work on any of the projects described below.

Contents of this page (the numbering is a continuation from the [M-Lab GSoC ideas page](http://measurementlab.net/gsoc_2012)):


## (3.1) Development of the library core ##

The libparistraceroute library is organized into a core and a set of modules. For a high level picture of how the library is organized, please see the [Overview](Overview.md) section. The first set of ideas concerns the core.

### (3.1.1) Complete the core ###

**Brief description**

The central development task, on which all other tasks are based, is to complete the development of the core. While it is already well advanced, work is still required to polish the base functionalities and tidy up the interface that the library offers to its users.

**Expected results**

A first release of libparistraceroute

**Knowledge prerequisite**

  * C programming
  * Development on Linux/UNIX
  * Basic knowledge of active network measurements

**Mentors**

  * [Jordan Augé](http://code.google.com/u/102658521840620528565/) (UPMC Sorbonne Universités)
  * [Marc-Olivier Buob](https://code.google.com/u/marcolivier.buob/) (UPMC Sorbonne Universités)

### (3.1.2) Build probes according to a set of constraints ###

**Brief description**

At present, the library offers a simple abstraction through which probe packets are constructed. It allows an application to describe the stack of protocols, assign values to fields, and provide the payload. The next step will be to allow applications to delegate more of the packet construction labor to the core. The application would simply define a set of _predicates_ that constrain the way that a probe packet is forged, and the core would do the rest.

For instance, suppose that we detect a load balancing router during a traceroute measurement (as indicated by an interface followed by several successor interfaces encountered during a trace towards a target destination). The router could be a per-flow load balancer, a per-packet load balancer, or a per-destination load balancer. In order to know whether a node corresponds to a per-packet load balancer, we must forge a set of probes belong to the same flow (same source IP, destination IP, source port, destination port and protocol). The constraints on these probes could be modeled using predicates.

There are several other applications that could be implemented thanks to predicates. A port scanning application is one example.

**Expected results**

A set of functions that allow to design easily any probe packet as easily as possible.

**Knowledge prerequisite**

  * C programming
  * Development on Linux/UNIX
  * Knowledge of networking and network protocols.

**Mentors**

  * [Jordan Augé](http://code.google.com/u/102658521840620528565/) (UPMC Sorbonne Universités)
  * [Marc-Olivier Buob](https://code.google.com/u/marcolivier.buob/) (UPMC Sorbonne Universités)

### (3.1.3) Develop schedulers and rate limits ###

**Brief description**

The library currently sends a packet immediately after it has been forged. We would like to be able to assign a precise sending time to each packet. Goals include allowing the sending of packets according to a given delay distribution, or allowing an application to replace the simple FIFO queue that is used for sending packets with a more complex scheduler. The mechanism could integrate a rate limiting process that would protect the user and the network by capping the amount of traffic being sent (during a time slot, towards a destination, etc.).

**Expected results**

A mechanism that schedule and regulate the probe packets sent.

**Knowledge prerequisite**

  * C programming
  * Development on Linux/UNIX

**Mentors**

  * [Jordan Augé](http://code.google.com/u/102658521840620528565/) (UPMC Sorbonne Universités)
  * [Marc-Olivier Buob](https://code.google.com/u/marcolivier.buob/) (UPMC Sorbonne Universités)

### (3.1.4) Enhance the performance of the library ###

**Brief description**

Currently, all the processing to craft and send probes is managed in a single thread. The idea is to make the following improvements to library performance:
  * allow multiple threads to coexist and make data structures used by the different algorithms thread-safe
  * introduce a preliminary phase during which a packet is built so that it can be sent more promptly at the required moment, or even sent many times

**Expected results**

A thread-safe design of the library.

**Knowledge prerequisite**

  * C programming (threads...)
  * Development on Linux/UNIX

**Mentors**

  * [Jordan Augé](http://code.google.com/u/102658521840620528565/) (UPMC Sorbonne Universités)
  * [Marc-Olivier Buob](https://code.google.com/u/marcolivier.buob/) (UPMC Sorbonne Universités)

## (3.2) Development of library modules ##

The organization of libparistraceroute around a set of modules, each one for an independent aspect, such as a protocol or an algorithm, makes the library highly flexible. The following are ideas for enlarging the scope of the library by bringing in new protocols and functionalities.

### (3.2.1) Development of protocol modules ###

**Brief description**

Through a protocol module, the library both crafts packets from a probe description, and determines which sent probe corresponds to a received probe reply. Currently, the library has IPv4, UDP, and ICMP modules, which together allow for the sending of UDP traceroute probes and the parsing of the corresponding ICMP replies. The idea is to develop modules that support IPv6, and/or TCP, which is more complex to handle.

**Expected results**

Support of IPv6 and TCP in libparistraceroute.

**Knowledge prerequisite**

  * C programming
  * Familiarity with IPv6 and TCP/IP.
  * Development on Linux/UNIX

**Mentors**

  * [Jordan Augé](http://code.google.com/u/102658521840620528565/) (UPMC Sorbonne Universités)
  * [Marc-Olivier Buob](https://code.google.com/u/marcolivier.buob/) (UPMC Sorbonne Universités)

### (3.2.2) Development of algorithm modules ###

**Brief description**

_Node and link queries_

Today, the library has two primitives for network graph discovery: "node query" and "link query". The former sends a single probe in order to discover a router interface, and the latter sends a pair of probes in order to discover two successive interfaces. These simple algorithms are building blocks from which network probing tools can be built.

The idea is to develop a richer toolbox of primitives of this sort. One would apply the essential Paris Traceroute principle of maintaining a constant flow identifier across multiple probes. Another would discover all links along a path following a given interface. Yet others remain to be defined.

_Multipath Discovery Algorithm (MDA)_

The Multipath Discovery Algorithm (MDA) allows Paris Traceroute to discover all paths between a source and a destination, bounding the failure probability under some basic assumptions and returning a significance level for the result. In the process, the algorithm identifies all per-flow, per-destination and per-packet load balancing routers along those paths.

The current MDA implementation in libparistraceroute would gain much in clarity if it were to be broken down into more basic bricks, such as node and link queries (mentioned above), modules that provide statistical tests, etc. This would open the way for additional statistical algorithms to be tried in Paris Traceroute.

**Expected results**

A set of useful algorithm modules (link query and node query for instance) that make libparistraceroute as user-friendly as possible.

**Knowledge prerequisite**

  * C programming
  * Familiarity with network measurement.
  * Development on Linux/UNIX

**Mentors**

  * [Jordan Augé](http://code.google.com/u/102658521840620528565/) (UPMC Sorbonne Universités)
  * [Marc-Olivier Buob](https://code.google.com/u/marcolivier.buob/) (UPMC Sorbonne Universités)

## (3.3) Provide Python and Ruby bindings ##

**Brief description**

Another idea would be to wrap libparistraceroute to provide Python and Ruby bindings and thus increase the visibility of the library. Ideally, these bindings should take advantage of particular language capabilities. For example, the Python binding might be inspired by [scapy](http://www.secdev.org/projects/scapy/).

**Expected results**

Python and/or ruby bindings.

**Knowledge prerequisite**

  * C programming
  * python/ruby programming
  * Development on Linux/UNIX

**Mentors**

  * [Jordan Augé](http://code.google.com/u/102658521840620528565/) (UPMC Sorbonne Universités)
  * [Marc-Olivier Buob](https://code.google.com/u/marcolivier.buob/) (UPMC Sorbonne Universités)

## (3.4) Packaging libparistraceroute ##

**Brief description**

Prepare a libparistraceroute-based Paris Traceroute command-line tool, of a quality sufficient to be integrated into one or more of the common variants of Linux. The current command-line tool is based on the earlier, monolithic, version of Paris Traceroute. The new tool should be thoroughly backwards-compatible, both at the command line and in the textual output, while also providing the new features that libparistraceroute offers, such as the ability to trace all the paths from a source to a destination using the MDA algorithm.  In addition, the command-line tool will need to be packaged for distribution. We're looking for someone here who already has a track record as a committer to a significant open source project, preferably involving Unix system code.

**Expected results**

A backward-compatible traceroute tool using libparistraceroute and the corresponding Debian and RPM packages.

**Knowledge prerequisite**

  * C programming
  * Development on Linux/UNIX
  * Basic knowledge of Linux packaging tools (APT and RPM).

**Mentors**

  * [Jordan Augé](http://code.google.com/u/102658521840620528565/) (UPMC Sorbonne Universités)
  * [Marc-Olivier Buob](https://code.google.com/u/marcolivier.buob/) (UPMC Sorbonne Universités)

## (3.5) Implementation of network measurement tools and algorithms ##

**Brief description**

There are several existing network measurement tools that are highly used but that do not yet benefit from the advancements of Paris Traceroute:
  * [Doubletree](http://inl.info.ucl.ac.be/tutorials/doubletree-way-reduce-redundancy-tracero)
  * [Reverse Traceroute ](http://www.cs.washington.edu/research/networking/astronomy/reverse-traceroute.html)
  * [MIDAR](http://www.caida.org/tools/measurement/midar/)
  * [Motu](http://www.caida.org/tools/measurement/motu/)
  * [iffinder](http://www.caida.org/tools/measurement/iffinder/)
  * [kapar](http://www.caida.org/tools/measurement/kapar/)
  * [scamper](http://www.wand.net.nz/scamper)
  * ...

The idea is to choose any one of these tools and to incorporate libparistraceroute, either into the existing tool or by reimplementing the tool, as appropriate.

There are several considerations to keep in mind:
  1. It could be a convenient way to improve existing tools and to test whether an improvement is promising.
  1. If libparistraceroute is well-designed, we should be able to optimize these tools or get comparable performance.
  1. Conducting this work will be helpful in defining the set of primitives that libparistraceroute needs to support. For example, the standard Traceroute tool measures a path towards a given target IP. Such a measurement can be seen as a "per-path" query. However, the MDA algorithm or [Doubletree](http://inl.info.ucl.ac.be/tutorials/doubletree-way-reduce-redundancy-tracero) aims at discovering paths from a set of source node towards a set of target destinations. "Per-path" queries would induce a large number of redundant measurements, because several paths share common subpaths. Thus, "per-node" and "per-link" queries are two primitives that significantly decrease the amount of probes used by these two algorithms.

**Expected results**

An implementation of these tools using libparistraceroute.

**Knowledge prerequisite**

  * C programming
  * Familiarity with network measurement.
  * Development on Linux/UNIX

**Mentors**

  * [Jordan Augé](http://code.google.com/u/102658521840620528565/) (UPMC Sorbonne Universités)
  * [Marc-Olivier Buob](https://code.google.com/u/marcolivier.buob/) (UPMC Sorbonne Universités)
  * [Timur Friedman](https://code.google.com/u/timur.friedman/) (UPMC Sorbonne Universités)

## (3.6) Develop distributed measurement techniques ##

**Brief description**

Typically, an active measurement tool enabled by libparistraceroute will be designed to run on a single machine. However, there are occasionally good reasons to design a distributed architecture for performing network measurements. We list below two applications that could be created thanks to libparistraceroute if it were to allow distributed measurements.

**Expected results**

A set of function in order to develop easily distributed algorithm thanks to libparistraceroute.

**Knowledge prerequisite**

  * C programming
  * Development on Linux/UNIX
  * Knowledge of networking and network protocols.

**Applications**

_Distributed traceroute measurements_

As we know from the Doubletree and MDA algorithms, distributed traceroute measurements can involve considerable probe redundancy, for instance when several traceroute paths cross the same set of interfaces. In these algorithms, measurement nodes share information that allow the overall system to reduce its level of redundant measurements. One can also imagine more sophisticated approaches, and  libparistraceroute should support them through its probe sender and response handler.

_IP/router aliasing problem_

The IP/router aliasing problem consists in discovering groups of IP addresses that all belong to the same router. [iffinder](http://www.caida.org/tools/measurement/iffinder/) is an example of a tool that tackles this problem. libparistraceroute could be used as the basis for a tool that solves this problem using a different approach.

Let us consider a A sender that emits a probe (for a given TTL and a given destination) towards a target T while spoofing the source IP address of receiver B. T will send the response to B. Thus, B collects the result of the probe sent by A and discovers an interface of T. We can iterate this approach by spoofing a set of IP addresses such that each address correspond to a a different receiver capable of handling T's responses. This would allow us to discover the set of interfaces belonging to T.

**Mentors**

  * [Jordan Augé](http://code.google.com/u/102658521840620528565/) (UPMC Sorbonne Universités)
  * [Marc-Olivier Buob](https://code.google.com/u/marcolivier.buob/) (UPMC Sorbonne Universités)
  * [Timur Friedman](https://code.google.com/u/timur.friedman/) (UPMC Sorbonne Universités)

## (3.7) Looking glass servers ##

**Brief description**

A final idea is the development and deployment of Looking Glass Servers for Paris Traceroute. There are two ways for people to benefit from the improved route tracing provided by Paris Traceroute: one is for them to use a Paris Traceroute command-line tool that is installed on their own operating system, and the other is to access a remotely-deployed instance of Paris Traceroute through a website or XML-RPC. There are already widely-deployed remote traceroute facilities throughout the world known as Looking Glass servers. The idea here is to create new Looking Glass Servers that incorporate the tracing improvements of Paris Traceroute. We would first deploy them on the global PlanetLab system. After testing and debugging, we would submit them for approval to be deployed on Measurement Lab, the PlanetLab system, sponsored by Google, that is dedicated to the testing of broadly-useful network measurement tools.


**Expected results**

A set of function in order to develop easily distributed algorithm thanks to libparistraceroute.

**Knowledge prerequisite**

  * Python programming
  * Development on Linux/UNIX

**Mentors**

  * [Jordan Augé](http://code.google.com/u/102658521840620528565/) (UPMC Sorbonne Universités)
  * [Marc-Olivier Buob](https://code.google.com/u/marcolivier.buob/) (UPMC Sorbonne Universités)
  * [Timur Friedman](https://code.google.com/u/timur.friedman/) (UPMC Sorbonne Universités)