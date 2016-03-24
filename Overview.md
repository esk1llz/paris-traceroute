# Overview of libparistraceroute #

This is a technical overview of the library that is at the core of [Paris Traceroute development](http://code.google.com/p/paris-traceroute/). For a scientific introduction to Paris Traceroute, please visit the [public website](http://www.paris-traceroute.net/). For ideas for development of the library, or for development that uses the library, please visit our [ideas page](Ideas.md).

## Introduction ##

libparistraceroute is more than simply the basis for the Paris Traceroute command-line tool. It is a general purpose library for any network measurement application that works in the style of traceroute or ping: sending probe packets and associating response packets with the initial probes. What gives libparistraceroute its "Paris Traceroute DNA" is the manner in which it handles flow identifiers, ensuring that probe packets follow the same path through the network in the face of per-destination and per-flow load balancing routers.

The library offers users the possibility to interact with it at various level of abstraction, from the low level basic packet crafting level, to a high level set of ready-to-use special purpose algorithms. For example, a tool that simply needs to trace a path towards a destination while taking into account load balancers will be able to request this information directly, while a tool that has more sophisticated needs will deploy its own algorithms on top of lower level library functionalities.

## Three levels of abstraction ##

libparistraceroute is split into three layers, as detailed below. Each one addresses a different need. For instance, the top layer allows a programmer to deploy a measurement algorithm without having to go into technical detail, whereas the bottom layer allows one to customize and tune the ways in which probes are forged. The choice of layer is up to the developer.

### Algorithm and topology layer ###

This top layer provides high-level primitives for network measurement. These primitives provide information for at the graph level (nodes, link, paths) while concealing underlying aspects (network protocol, etc.).

[tracetree](http://content.lip6.fr/latapy/Radar/Programs/README.tracetree) is an example of the sort of tool that could be deployed on top of this layer. It conducts traceroute-like probes, but backwards, starting at the destination and sending successive probes over ever shorter distances. It is designed to trace towards large numbers of destinations in this fashion, and the backwards tracing stops when it encounters interfaces that it has already seen. By so doing, it avoids redundancy. The end result is a tree of traces, rooted at the source, with the leaves at the destinations.

Note that tracetree does not need to focus on the ways in which probe packets are forged. For its algorithm, the useful primitives are a query that estimates the path length toward a given destination, to begin probing, and a query that returns the identity of the interface revealed by probing towards a given destination with a given hop count.

### Probe and network layer ###

This middle layer provides a set of functions and structures that allow an application to easily build a wide variety of probe packets. The motivation for this layer is similar to that of [Scapy](http://www.secdev.org/projects/scapy/): we cannot tell ahead of time which kinds of probes a developer might need to generate.

This layer forges probes by combining different pre-defined protocol headers (for example IPv4, TCP, ICMP, and UDP headers), along with a payload. There is a dedicated module for each protocol that it supports. The library is also architected in such a way that a developer may easily incorporate custom modules.

This layer also allows the developer to tune probes for specific needs. For example, _IP spoofing_ consists of sending a packet with a source IP address that does not correspond to the sender. Spoofing and other such techniques can easily be carried out thanks to this layer. If the source address is omitted, the probe will use the correct address.

### Packet and socket layer ###

This bottom layer provides the greatest possible flexibility in packet forging and in sniffing responses to probe packets. Whereas the probe and network layer handles the details of putting identifiers into probes and matching responses to the probes that generated them, the packet and socket layer allows an application to fully specify these aspects.

Since an application might run several network probing algorithms at the same moment, this layer also provides functionality for demultiplexing responses to the correct originating algorithm.

## Characteristics of the library ##

libparistraceroute is written in C and is made available under a New BSD licence. Development plans call for Python and Ruby bindings for the library and for the library to be packaged for common Linux distributions. Our [ideas page](Ideas.md) describes directions for further development.