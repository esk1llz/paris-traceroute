# Introduction #

Add your content here.


# Details #

## Some open questions ##

Initial, basic questions:
  * How does ICMP6 encapsulate IPv6 packet headers when sending a TTL time exceeded packet?

It does include as much of the packet as possible. Basicly same format as ICMP.

  * Which IPv6 packet headers can be used to uniquely tag probe packets, so that they can be matched with ICMP6 replies?

In the standard IPv6 header there might be the flowlabel (20 bits) most useful. Other than that we might vary source addresses (64 bits). Furthermore extension headers might get used, destination extension headers might be a viable way.


  * Which IPv6 header fields do load balancing routers use? Are these different from those used for load balancing IPv4 packets?

More advanced questions:
  * When tunneling IPv6 across IPv4 networks, might any aspect of these tunnels be visible and affect how traceroutes take place, or will they be fully transparent. (Our experience in IPv4 with MPLS has indicated that MPLS tunnels are not always fully transparent to traceroute.)
  * When running IPv6 traceroute over 6LoWPAN, are there any aspects of 6LoWPAN header compression and gateways that would interfere with the normal operation of traceroute?

## Related tools to look at ##

  * [Scamper](http://www.wand.net.nz/~mluckie/ipv6-scamper/)


## References ##

  * "IPv6 Flow Label Specification" [RFC 6437](http://tools.ietf.org/html/rfc6437) (obsoletes [RFC 3697](http://tools.ietf.org/html/rfc3697))


## Testing environments ##

Possible environments for testing lipparistraceroute over IPv6 include:
  * Actual campus-level IPv6 networks
  * [Fakeroute](Fakeroute.md)
  * [GNS3](http://www.gns3.net/) network emulation environment with the [Dynamips](http://dynagen.org/) router emulator from Cisco

## Workplan ##

Initial
  * Create an IPv6 protocol module
    * Description of header contents: list the different headers with their size and offset
    * Write helper functions to convert between strings and the header and the header and strings
    * Specify the matchings between packets that are sent and possible replies (IPv6 replies or ICMP6 replies)
  * Add the ability to handle IPv6 sockets to the socket pool manager (the socket pool module) in the network layer
  * Modify the received packet identification mechanism to identify IPv6 packets and forward them to the IPv6 protocol module

Later
  * Implement flow modules that describe the flow metafield for UDP/IPv6, TCP/IPv6, ICMP6/IPv6
  * Test the IPv6 implementation over 6LoWPAN
  * Test the IPv6 implementation over IPv4 tunnels