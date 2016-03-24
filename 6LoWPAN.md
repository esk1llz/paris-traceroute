# Introduction #

How to deploy libparistraceroute on machines that are inside 6LoWPAN environments.


# Details #

Work here depends upon the [IPv6](IPv6.md) module having been enabled. We will then test how traceroutes work when sending packets across a 6LoWPAN domain. Then, we focus here on how to directly generate and receive 6LoWPAN packets on a machine that is inside a 6LoWPAN environment.

## Testing environments ##

  * Cooja is a network simulator that comes with Contiki OS. It can emulate a WSN environment. It is primarily developed for Contiki OS, and it emulates Contiki-based machines on an instruction level, so down to the timing details. It is also possible to emulate TinyOS on Cooja.
  * USB networking devices that attach to a Linux box and that run 802.15.4, allowing one to inject packets into a 6LoWPAN environment.

## Issues to be addressed ##

  * We are not sure if we can rely upon 6LoWPAN compression and expansion to be completely transparent. Is it possible that information might get lost? For instance, if there is fragmentation, is it possible that we would just receive the first fragment of a reply? We do not expect this to happen, but we need to check on this.

## Workplan ##

  * Create a 6LoWPAN protocol module
    * Description of header contents: list the different headers with their size and offset
    * Write helper functions to convert between strings and the header and the header and strings
    * Specify the matchings between packets that are sent and possible replies
  * Add the ability to handle 6LoWPAN sockets (if these are distinct from standard IPv6 sockets) to the socket pool manager (the socket pool module) in the network layer
  * Modify the received packet identification mechanism to identify 6LoWPAN packets and forward them to the 6LoWPAN protocol module