# Backward compatibility of input with Modern Traceroute for Linux #

People are more likely to adopt Paris Traceroute as a replacement for classic traceroute if its command line arguments are exactly identical to those of the classic version. Our reference for backwards compatibility is Dmitry Butskoy's [Modern Traceroute for Linux](http://traceroute.sourceforge.net/), which is found on many Linux distributions today (Fedora, Debian, Ubuntu...). The most recent version is 2.0.19, of 6 December 2012.

# Objectives and status #

The following table lists each command line option of Modern Traceroute for Linux 2.0.19, our objectives with regards to the option in Paris Traceroute, and the current status of development. There is a link from each option to a detailed discussion of that option.

Objectives meanings:
  * Phase 2: required in order to be able to deprecate the C++ version of Paris Traceroute
  * Phase 3: required in order to have a Paris Traceroute that will gain wide acceptance as a Modern Traceroute replacement
  * Phase 4: required in order to be fully backward compatible with Modern Traceroute

Status meanings:
  * verify current status:
    * verify that the parameter is parsed correctly at the command line
    * verify that the output is identical to the classic traceroute output

| **option** | **objectives** | **status** |
|:-----------|:---------------|:-----------|
| [-4](#-4.md) | Phase 2        | verify that it is parsed on the command line |
| [-6](#6.md) | Phase 3        | under development |
| [-d](#-d.md) --debug | Phase 3        |            |
| [-F](#-F.md) --dont-fragment | Phase 4        |
| [-f](#-f.md) first\_ttl  --first=first\_ttl | Phase 2        | verify current status |
| [-g](#-g.md) gate,...  --gateway=gate,... | Phase 4        |
| [-I](#-I.md)  --icmp | Phase 3        | not developed |
| [-T](#-T.md)  --tcp | Phase 3        | verify current status |
| [-i](#-i.md) device --interface=device | Phase 3        |
| [-m](#-m.md) max\_ttl  --max-hops=max\_ttl | Phase 2        | verify current status |
| [-N](#-N.md) squeries  --sim-queries=squeries | Phase 4        |
| [-n](#-n.md) | Phase 2        |
| [-p](#-p.md) port  --port=port | Phase 3        | verify current status |
| [-t](#-t.md) tos  --tos=tos | Phase 4        | verify current status |
| [-l](#-l.md) flow\_label  --flowlabel=flow\_label| Phase 4        | to be discussed |
| [-w](#-w.md) waittime  --wait=waittime | Phase 2        |
| [-q](#-q.md) nqueries  --queries=nqueries| Phase 4        |
| [-r](#-r.md) | Phase 4        |
| [-s](#-s.md) src\_addr  --source=src\_addr| Phase 3        | verify current status |
| [-z](#-z.md) sendwait  --sendwait=sendwait| Phase 3        |
| [-e](#-e.md)  --extensions| Phase 4        |
| [-A](#-A.md)  --as-path-lookups | Phase 4        |            |
| [-M](#-M.md) name  --module=name | Phase 4        |
| [-O](#-O.md) OPTS,...  --options=OPTS,... | Phase 4        |
| [--sport](#--sport.md)=num | Phase 4        | verify current status |
| [--fwmark](#--fwmark.md)=num | Phase 4        |
| [-U](#-U.md)  --udp| Phase 2        | verify current status |
| [-UL](#-UL.md) | Phase 4?       |
| [-D](#-D.md) | Phase 4        |
| [-P](#-P.md) prot  --protocol=prot | Phase 2        | to be discussed |
| [--mtu](#--mtu.md) | Phase 4        |
| [--back](#--back.md) | Phase 4        |
| [-V](#-V.md)  --version | Phase 2        | to be implemented |
| [--help](#--help.md) | Phase 2        | to be implemented |


# Target version of classic traceroute #

We aim to be backward compatible with Dmitry Butskoy's Modern Traceroute for Linux ([home page](http://traceroute.sourceforge.net/), [SourceForge project page](http://sourceforge.net/projects/traceroute/), [download](http://sourceforge.net/projects/traceroute/files/), [mailing list](https://lists.sourceforge.net/lists/listinfo/traceroute-devel)). This is the version of classic traceroute found on many Linux distributions today (Fedora, Debian, Ubuntu...).

```
$ traceroute --version
Modern traceroute for Linux, version 2.0.19, Dec 6 2012
Copyright (c) 2008  Dmitry Butskoy,   License: GPL v2 or any later
$
```

```
$ traceroute          
Usage:
  traceroute [ -46dFITUnreAV ] [ -f first_ttl ] [ -g gate,... ] [ -i device ] [ -m max_ttl ] [ -p port ] [ -s src_addr ]
  [ -q nqueries ] [ -N squeries ] [ -t tos ] [ -l flow_label ] [ -w waittime ] [ -z sendwait ] [ -UL ] [ -D ] [ -P proto ]
  [ --sport=port ] [ -M method ] [ -O mod_options ] [ --mtu ] [ --back ] host [ packet_len ]
Options:
  -4                          Use IPv4
  -6                          Use IPv6
  -d  --debug                 Enable socket level debugging
  -F  --dont-fragment         Do not fragment packets
  -f first_ttl  --first=first_ttl
                              Start from the first_ttl hop (instead from 1)
  -g gate,...  --gateway=gate,...
                              Route packets through the specified gateway
                              (maximum 8 for IPv4 and 127 for IPv6)
  -I  --icmp                  Use ICMP ECHO for tracerouting
  -T  --tcp                   Use TCP SYN for tracerouting
  -i device  --interface=device
                              Specify a network interface to operate with
  -m max_ttl  --max-hops=max_ttl
                              Set the max number of hops (max TTL to be
                              reached). Default is 30
  -N squeries  --sim-queries=squeries
                              Set the number of probes to be tried
                              simultaneously (default is 16)
  -n                          Do not resolve IP addresses to their domain names
  -p port  --port=port        Set the destination port to use. It is either
                              initial udp port value for "default" method
                              (incremented by each probe, default is 33434), or
                              initial seq for "icmp" (incremented as well,
                              default from 1), or some constant destination
                              port for other methods (with default of 80 for
                              "tcp", 53 for "udp", etc.)
  -t tos  --tos=tos           Set the TOS (IPv4 type of service) or TC (IPv6
                              traffic class) value for outgoing packets
  -l flow_label  --flowlabel=flow_label
                              Use specified flow_label for IPv6 packets
  -w waittime  --wait=waittime
                              Set the number of seconds to wait for response to
                              a probe (default is 5.0). Non-integer (float
                              point) values allowed too
  -q nqueries  --queries=nqueries
                              Set the number of probes per each hop. Default is
                              3
  -r                          Bypass the normal routing and send directly to a
                              host on an attached network
  -s src_addr  --source=src_addr
                              Use source src_addr for outgoing packets
  -z sendwait  --sendwait=sendwait
                              Minimal time interval between probes (default 0).
                              If the value is more than 10, then it specifies a
                              number in milliseconds, else it is a number of
                              seconds (float point values allowed too)
  -e  --extensions            Show ICMP extensions (if present), including MPLS
  -A  --as-path-lookups       Perform AS path lookups in routing registries and
                              print results directly after the corresponding
                              addresses
  -M name  --module=name      Use specified module (either builtin or external)
                              for traceroute operations. Most methods have
                              their shortcuts (`-I' means `-M icmp' etc.)
  -O OPTS,...  --options=OPTS,...
                              Use module-specific option OPTS for the
                              traceroute module. Several OPTS allowed,
                              separated by comma. If OPTS is "help", print info
                              about available options
  --sport=num                 Use source port num for outgoing packets. Implies
                              `-N 1'
  --fwmark=num                Set firewall mark for outgoing packets
  -U  --udp                   Use UDP to particular port for tracerouting
                              (instead of increasing the port per each probe),
                              default port is 53
  -UL                         Use UDPLITE for tracerouting (default dest port
                              is 53)
  -D  --dccp                  Use DCCP Request for tracerouting (default
                              port is 33434)
  -P prot  --protocol=prot    Use raw packet of protocol prot for tracerouting
  --mtu                       Discover MTU along the path being traced. Implies
                              `-F -N 1'
  --back                      Guess the number of hops in the backward path and
                              print if it differs
  -V  --version               Print version info and exit
  --help                      Read this help and exit

Arguments:
+     host          The host to traceroute to
      packetlen     The full packet length (default is the length of an IP
                    header plus 40). Can be ignored or increased to a minimal
                    allowed value
$
```

## Manual page ##

```
TRACEROUTE(8)                Traceroute For Linux                TRACEROUTE(8)

NAME
       traceroute - print the route packets trace to network host

SYNOPSIS
       traceroute [-46dFITUnreAV] [-f first_ttl] [-g gate,...]
               [-i device] [-m max_ttl] [-p port] [-s src_addr]
               [-q nqueries] [-N squeries] [-t tos]
               [-l flow_label] [-w waittime] [-z sendwait] [-UL] [-D]
               [-P proto] [--sport=port] [-M method] [-O mod_options]
               [--mtu] [--back]
               host [packet_len]
       traceroute6  [options]

DESCRIPTION
       traceroute  tracks  the route packets taken from an IP network on their
       way to a given host. It utilizes the IP protocol's time to  live  (TTL)
       field  and  attempts to elicit an ICMP TIME_EXCEEDED response from each
       gateway along the path to the host.

       traceroute6 is equivalent to traceroute -6

       The only required parameter is the name or IP address of  the  destina-
       tion host .  The optional packet_len`gth is the total size of the prob-
       ing packet (default 60 bytes for IPv4 and 80 for IPv6).  The  specified
       size  can  be  ignored  in some situations or increased up to a minimal
       value.

       This program attempts to trace the route an IP packet would  follow  to
       some internet host by launching probe packets with a small ttl (time to
       live) then listening for an ICMP "time exceeded" reply from a  gateway.
       We  start our probes with a ttl of one and increase by one until we get
       an ICMP "port unreachable" (or TCP reset), which means we  got  to  the
       "host",  or  hit  a  max  (which defaults to 30 hops). Three probes (by
       default) are sent at each ttl setting and a line is printed showing the
       ttl,  address  of  the  gateway  and round trip time of each probe. The
       address can be followed by additional information  when  requested.  If
       the  probe  answers  come  from different gateways, the address of each
       responding system will be printed.  If there is no  response  within  a
       5.0 seconds (default), an "*" (asterisk) is printed for that probe.

       After the trip time, some additional annotation can be printed: !H, !N,
       or !P  (host,  network  or  protocol  unreachable),  !S  (source  route
       failed),  !F (fragmentation needed), !X (communication administratively
       prohibited), !V (host precedence violation), !C (precedence  cutoff  in
       effect),  or  !<num>  (ICMP unreachable code <num>).  If almost all the
       probes result in some kind of unreachable, traceroute will give up  and
       exit.

       We don't want the destination host to process the UDP probe packets, so
       the destination port is set to an unlikely value  (you  can  change  it
       with  the  -p flag). There is no such a problem for ICMP or TCP tracer-
       outing (for TCP we use half-open technique, which prevents  our  probes
       to be seen by applications on the destination host).

       In  the  modern  network environment the traditional traceroute methods
       can not be always applicable, because of widespread use  of  firewalls.
       Such  firewalls  filter  the "unlikely" UDP ports, or even ICMP echoes.
       To solve this, some additional  tracerouting  methods  are  implemented
       (including  tcp), see LIST OF AVAILABLE METHODS below. Such methods try
       to use particular protocol and source/destination  port,  in  order  to
       bypass  firewalls  (to  be seen by firewalls just as a start of allowed
       type of a network session).

OPTIONS
       --help Print help info and exit.

       -4, -6 Explicitly force IPv4 or IPv6 tracerouting. By default, the pro-
              gram  will  try to resolve the name given, and choose the appro-
              priate protocol automatically. If resolving a host name  returns
              both IPv4 and IPv6 addresses, traceroute will use IPv4.

       -I     Use ICMP ECHO for probes

       -T     Use TCP SYN for probes

       -d     Enable  socket  level  debugging (when the Linux kernel supports
              it)

       -F     Do not fragment probe packets. (For IPv4 it also  sets  DF  bit,
              which  tells  intermediate  routers  not to fragment remotely as
              well).

              Varying the size of the probing packet by the packet_len command
              line  parameter,  you  can manually obtain information about the
              MTU of individual network hops. The  --mtu  option  (see  below)
              tries to do this automatically.

              Note, that non-fragmented features (like -F or --mtu) work prop-
              erly since the Linux kernel 2.6.22 only.  Before  that  version,
              IPv6  was always fragmented, IPv4 could use the once the discov-
              ered final mtu only (from the route cache), which  can  be  less
              than the actual mtu of a device.

       -f first_ttl
              Specifies with what TTL to start. Defaults to 1.

       -g gateway
              Tells  traceroute to add an IP source routing option to the out-
              going packet that tells the network to route the packet  through
              the specified gateway (most routers have disabled source routing
              for security reasons).  In general, several gateway's is allowed
              (comma  separated).  For  IPv6, the form of num,addr,addr...  is
              allowed, where num is a route header type (default is  type  2).
              Note the type 0 route header is now deprecated (rfc5095).

       -i interface
              Specifies  the  interface  through  which traceroute should send
              packets. By default, the interface is selected according to  the
              routing table.

       -m max_ttl
              Specifies  the  maximum  number of hops (max time-to-live value)
              traceroute will probe. The default is 30.

       -N squeries
              Specifies the number of probe packets sent  out  simultaneously.
              Sending several probes concurrently can speed up traceroute con-
              siderably. The default value is 16.
              Note that some routers and hosts can use ICMP  rate  throttling.
              In such a situation specifying too large number can lead to loss
              of some responses.

       -n     Do not try to map IP addresses to  host  names  when  displaying
              them.

       -p port
              For  UDP tracing, specifies the destination port base traceroute
              will use (the destination port number  will  be  incremented  by
              each probe).
              For  ICMP  tracing,  specifies  the  initial ICMP sequence value
              (incremented by each probe too).
              For TCP and others specifies  just  the  (constant)  destination
              port to connect.

       -t tos For  IPv4,  set  the Type of Service (TOS) and Precedence value.
              Useful values are 16 (low delay) and 8 (high  throughput).  Note
              that  in order to use some TOS precedence values, you have to be
              super user.
              For IPv6, set the Traffic Control value.

       -l flow_label
              Use specified flow_label for IPv6 packets.

       -w waittime
              Set the time (in seconds) to wait for  a  response  to  a  probe
              (default 5.0 sec).

       -q nqueries
              Sets the number of probe packets per hop. The default is 3.

       -r     Bypass  the normal routing tables and send directly to a host on
              an attached network.  If the host is not on a  directly-attached
              network,  an error is returned.  This option can be used to ping
              a local host through an interface that has no route through it.

       -s source_addr
              Chooses an alternative source address. Note that you must select
              the  address  of one of the interfaces.  By default, the address
              of the outgoing interface is used.

       -z sendwait
              Minimal time interval between probes (default 0).  If the  value
              is  more  than  10,  then it specifies a number in milliseconds,
              else it is a number of seconds (float point values allowed too).
              Useful when some routers use rate-limit for ICMP messages.

       -e     Show  ICMP extensions (rfc4884). The general form is CLASS/TYPE:
              followed by a hexadecimal dump.  The  MPLS  (rfc4950)  is  shown
              parsed,  in  a form: MPLS:L=label,E=exp_use,S=stack_bottom,T=TTL
              (more objects separated by / ).

       -A     Perform AS path lookups in routing registries and print  results
              directly after the corresponding addresses.

       -V     Print the version and exit.

       There is a couple of additional options, intended for an advanced usage
       (another trace methods etc.):

       --sport=port
              Chooses the source port to use. Implies -N 1.   Normally  source
              ports (if applicable) are chosen by the system.

       --fwmark=mark
              Set the firewall mark for outgoing packets (since the Linux ker-
              nel 2.6.25).

       -M method
              Use specified method for traceroute operations.  Default  tradi-
              tional  udp method has name default, icmp (-I) and tcp (-T) have
              names icmp and tcp respectively.
              Method-specific options can be passed by -O .  Most methods have
              their simple shortcuts, (-I means -M icmp, etc).

       -O option
              Specifies some method-specific option. Several options are sepa-
              rated by comma (or use several -O on cmdline).  Each method  may
              have its own specific options, or many not have them at all.  To
              print information about available options, use -O help.

       -U     Use UDP to particular destination port for tracerouting (instead
              of  increasing  the  port  per  each  probe). Default port is 53
              (dns).

       -UL    Use UDPLITE for tracerouting (default port is 53).

       -D     Use DCCP Requests for probes.

       -P protocol
              Use raw packet of specified protocol for  tracerouting.  Default
              protocol is 253 (rfc3692).

       --mtu  Discover  MTU along the path being traced. Implies -F -N 1.  New
              mtu is printed once in a form of F=NUM at the first probe  of  a
              hop which requires such mtu to be reached. (Actually, the corre-
              spond "frag needed" icmp message normally is sent by the  previ-
              ous hop).

              Note, that some routers might cache once the seen information on
              a fragmentation. Thus you can  receive  the  final  mtu  from  a
              closer hop.  Try to specify an unusual tos by -t , this can help
              for one attempt (then it can be cached there as well).
              See -F option for more info.

       --back Print the number of backward hops when it seems  different  with
              the forward direction. This number is guessed in assumption that
              remote hops send reply packets with initial ttl  set  to  either
              64, or 128 or 255 (which seems a common practice). It is printed
              as a negate value in a form of '-NUM' .

LIST OF AVAILABLE METHODS
       In general, a particular traceroute method may have  to  be  chosen  by
       -M name,  but  most  of  the methods have their simple cmdline switches
       (you can see them after the method name, if present).

   default
       The traditional, ancient method of tracerouting. Used by default.

       Probe packets are udp datagrams with so-called  "unlikely"  destination
       ports.   The "unlikely" port of the first probe is 33434, then for each
       next probe it is incremented by one. Since the ports are expected to be
       unused,  the destination host normally returns "icmp unreach port" as a
       final response.  (Nobody knows what happens when some application  lis-
       tens for such ports, though).

       This method is allowed for unprivileged users.

   icmp       -I
       Most usual method for now, which uses icmp echo packets for probes.
       If  you can ping(8) the destination host, icmp tracerouting is applica-
       ble as well.

       This method may be allowed for unprivileged users since the kernel  3.0
       (IPv4  only),  which  supports  new  dgram icmp (or "ping") sockets. To
       allow such sockets, sysadmin should  provide  net/ipv4/ping_group_range
       sysctl range to match any group of the user.
       Options:

       raw    Use only raw sockets (the traditional way).
              This  way is tried first by default (for compatibility reasons),
              then new dgram icmp sockets as fallback.

       dgram  Use only dgram icmp sockets.

   tcp        -T
       Well-known modern method, intended to bypass firewalls.
       Uses the constant destination port (default is 80, http).

       If some filters are present in the network path, then most probably any
       "unlikely"  udp  ports  (as for default method) or even icmp echoes (as
       for icmp) are filtered, and whole tracerouting will just stop at such a
       firewall.  To bypass a network filter, we have to use only allowed pro-
       tocol/port combinations. If we trace for some,  say,  mailserver,  then
       more likely -T -p 25 can reach it, even when -I can not.

       This  method  uses  well-known  "half-open  technique",  which prevents
       applications on the destination host from seeing  our  probes  at  all.
       Normally,  a  tcp  syn  is  sent. For non-listened ports we receive tcp
       reset, and all is done. For  active  listening  ports  we  receive  tcp
       syn+ack,  but  answer  by tcp reset (instead of expected tcp ack), this
       way the remote tcp session is dropped even without the application ever
       taking notice.

       There is a couple of options for tcp method:

       syn,ack,fin,rst,psh,urg,ece,cwr
              Sets specified tcp flags for probe packet, in any combination.

       flags=num
              Sets the flags field in the tcp header exactly to num.

       ecn    Send syn packet with tcp flags ECE and CWR (for Explicit Conges-
              tion Notification, rfc3168).

       sack,timestamps,window_scaling
              Use the corresponding tcp header option in  the  outgoing  probe
              packet.

       sysctl Use  current sysctl (/proc/sys/net/*) setting for the tcp header
              options above and ecn.  Always set by default, if  nothing  else
              specified.

       mss=num
              Use value of num for maxseg tcp header option (when syn).

       info   Print  tcp  flags  of  final tcp replies when the target host is
              reached.  Allows to determine whether an application listens the
              port and other useful things.

       Default options is syn,sysctl.

   tcpconn
       An  initial implementation of tcp method, simple using connect(2) call,
       which does full tcp session opening. Not recommended  for  normal  use,
       because  a  destination application is always affected (and can be con-
       fused).

   udp        -U
       Use udp datagram with constant destination port (default 53, dns).
       Intended to bypass firewall as well.

       Note, that unlike in tcp method, the correspond application on the des-
       tination  host  always  receive our probes (with random data), and most
       can easily be confused by them. Most cases it will not respond  to  our
       packets  though, so we will never see the final hop in the trace. (For-
       tunately, it seems that at least dns  servers  replies  with  something
       angry).

       This method is allowed for unprivileged users.

   udplite    -UL
       Use  udplite  datagram  for  probes  (with  constant  destination port,
       default 53).

       This method is allowed for unprivileged users.
       Options:

       coverage=num
              Set udplite send coverage to num.

   dccp    -D
       Use DCCP Request packets for probes (rfc4340).

       This method uses the same "half-open technique" as used for  TCP.   The
       default destination port is 33434.

       Options:

       service=num
              Set DCCP service code to num (default is 1885957735).

   raw        -P proto
       Send raw packet of protocol proto.
       No protocol-specific headers are used, just IP header only.
       Implies -N 1.
       Options:

       protocol=proto
              Use IP protocol proto (default 253).

NOTES
       To  speed up work, normally several probes are sent simultaneously.  On
       the other hand, it creates a "storm of  packages",  especially  in  the
       reply  direction.  Routers can throttle the rate of icmp responses, and
       some of replies can be lost. To avoid  this,  decrease  the  number  of
       simultaneous  probes,  or  even set it to 1 (like in initial traceroute
       implementation), i.e.  -N 1

       The final (target) host can drop some of the simultaneous  probes,  and
       might  even  answer  only  the latest ones. It can lead to extra "looks
       like expired" hops near the final hop. We  use  a  smart  algorithm  to
       auto-detect  such a situation, but if it cannot help in your case, just
       use -N 1 too.

       For even greater stability you can slow down the program's work  by  -z
       option, for example use -z 0.5 for half-second pause between probes.

       If some hops report nothing for every method, the last chance to obtain
       something is to use ping -R command  (IPv4,  and  for  nearest  8  hops
       only).

SEE ALSO
       ping(8), ping6(8), tcpdump(8), netstat(8)

Traceroute                      11 October 2006                  TRACEROUTE(8)
```

# Discussion of options #

## -4 ##

```
  -4                          Use IPv4
```

Of course, Paris Traceroute must support IPv4, so this is for Phase 2.

## -6 ##

```
  -6                          Use IPv6
```

Not essential for replacing the C++ version of Paris Traceroute, so this is for Phase 3.

## -d ##

```
  -d  --debug                 Enable socket level debugging
```

Verbose output is a very standard feature, but not required for replacing the C++ version of Paris Traceroute, so this is for Phase 3.

## -F ##

```
  -F  --dont-fragment         Do not fragment packets
```

This seems like a bit of an obscure option, so it is for Phase 4.

## -f ##

```
  -f first_ttl  --first=first_ttl
                              Start from the first_ttl hop (instead from 1)
```

This is a very standard option, required for replacing the C++ version of Paris Traceroute, so this is for Phase 2.

## -g ##

```
  -g gate,...  --gateway=gate,...
                              Route packets through the specified gateway
                              (maximum 8 for IPv4 and 127 for IPv6)
```

This is important on multi-homed hosts, but perhaps a bit obscure. Phase 4.

## -I ##

```
  -I  --icmp                  Use ICMP ECHO for tracerouting
```

It will be important for us to support ICMP and TCP, but not required for replacement of the C++ version of Paris Traceroute, so for Phase 3.

## -T ##

```
  -T  --tcp                   Use TCP SYN for tracerouting
```

## -i ##

```
  -i device  --interface=device
                              Specify a network interface to operate with
```

This is important on multi-homed hosts.

## -m ##

```
  -m max_ttl  --max-hops=max_ttl
                              Set the max number of hops (max TTL to be
                              reached). Default is 30
```

## -N ##

```
  -N squeries  --sim-queries=squeries
                              Set the number of probes to be tried
                              simultaneously (default is 16)
```

## -n ##

```
  -n                          Do not resolve IP addresses to their domain names
```

We actually need to implement resolution of IP addresses in Phase 2, so -n would also be Phase 2.

## -p ##

```
  -p port  --port=port        Set the destination port to use. It is either
                              initial udp port value for "default" method
                              (incremented by each probe, default is 33434), or
                              initial seq for "icmp" (incremented as well,
                              default from 1), or some constant destination
                              port for other methods (with default of 80 for
                              "tcp", 53 for "udp", etc.)
```

This is a very basic function, but not required for replacing the C++ version of Paris Traceroute, so should be available in Phase 3.

With the MDA, it will just set a constant destination port number to use. In the library-based version of Paris Traceroute, the MDA does not vary the destination port number, so as to avoid having probing look like a port scan attack. So this option is compatible with the MDA.

## -t ##

```
  -t tos  --tos=tos           Set the TOS (IPv4 type of service) or TC (IPv6
                              traffic class) value for outgoing packets
```

Somewhat obscure option. Phase 4.

## -l ##

```
  -l flow_label  --flowlabel=flow_label
                              Use specified flow_label for IPv6 packets
```

Obscure option. Phase 4.

## -w ##

```
  -w waittime  --wait=waittime
                              Set the number of seconds to wait for response to
                              a probe (default is 5.0). Non-integer (float
                              point) values allowed too
```

This is part of the C++ version of Paris Traceroute. Phase 2.

## -q ##

```
  -q nqueries  --queries=nqueries
                              Set the number of probes per each hop. Default is
                              3
```

Most people use the default of 3 probes per hop. So Phase 4.

## -r ##

```
  -r                          Bypass the normal routing and send directly to a
                              host on an attached network
```

Unsure about exactly what this option does. Seems obscure and for Phase 4. To double-check.

## -s ##

```
  -s src_addr  --source=src_addr
                              Use source src_addr for outgoing packets
```

This is important on multi-homed hosts. Not a feature of the C++ version of Paris Traceroute, so Phase 3.

## -z ##

```
  -z sendwait  --sendwait=sendwait
                              Minimal time interval between probes (default 0).
                              If the value is more than 10, then it specifies a
                              number in milliseconds, else it is a number of
                              seconds (float point values allowed too)
```

This corresponds to a feature of the C++ version of Paris Traceroute, but not a key feature. Will be useful, so Phase 3.

## -e ##

```
  -e  --extensions            Show ICMP extensions (if present), including MPLS
```

This is a nice feature to have, but not essential. It is something that the C++ version of Paris Traceroute does. So Phase 4.

## -A ##

```
  -A  --as-path-lookups       Perform AS path lookups in routing registries and
                              print results directly after the corresponding
                              addresses
```

Not done by the C++ version. Also, we have strong doubts that it is required for any version of traceroute. So not even for Phase 4.

## -M ##

```
  -M name  --module=name      Use specified module (either builtin or external)
                              for traceroute operations. Most methods have
                              their shortcuts (`-I' means `-M icmp' etc.)
```

To be backward compatible, should support. Not in the C++ version of Paris Traceroute. Phase 4.

## -O ##

```
  -O OPTS,...  --options=OPTS,...
                              Use module-specific option OPTS for the
                              traceroute module. Several OPTS allowed,
                              separated by comma. If OPTS is "help", print info
                              about available options
```

To be backward compatible, should support. Not in the C++ version of Paris Traceroute. Phase 4.

## --sport ##

```
  --sport=num                 Use source port num for outgoing packets. Implies
                              `-N 1'
```

To be backward compatible, should support. Not in the C++ version of Paris Traceroute. Phase 4.

## --fwmark ##

```
  --fwmark=num                 Set firewall mark for outgoing packets
```

To be backward compatible, should support. Not in the C++ version of Paris Traceroute. Phase 4, or not at all if too obscure.

## -U ##

```
  -U  --udp                   Use UDP to particular port for tracerouting
                              (instead of increasing the port per each probe),
                              default port is 53
```

To be further discussed.

## -UL ##

```
  -UL                         Use UDPLITE for tracerouting (default dest port
                              is 53)
```

To be further discussed.

## -D ##

```
  -D  --dccp                  Use DCCP Request for tracerouting (default
                              port is 33434)
```

This is a very recent addition to Modern Traceroute for Linux. Phase 4.

## -P ##

```
  -P prot  --protocol=prot    Use raw packet of protocol prot for tracerouting
```

This will be a valid option, but it will not do anything since we always use raw packets. Phase 2.

To study: a means of not always using raw packets. Phase 4.

## --mtu ##

```
  --mtu                       Discover MTU along the path being traced. Implies
                              `-F -N 1'
```

To be backward compatible, should support. Not in the C++ version of Paris Traceroute. Phase 4.

## --back ##

```
  --back                      Guess the number of hops in the backward path and
                              print if it differs
```

To be backward compatible, should support. Not in the C++ version of Paris Traceroute. Phase 4.

## -V ##

```
  -V  --version               Print version info and exit
```

Phase 2.

## --help ##

```
  --help                      Read this help and exit
```

Phase 2.


| **Phase 2** |
|:------------|
| -4          |
| -f          |
| -n          |
| -w          |
| -P          |
| -V          |
| --help      |
| **Phase 3** |
| -6          |
| -d          |
| -I          |
| -p          |
| -s          |
| -z          |
| **Phase 4** |
| -F          |
| -g          |
| -t          |
| -I          |
| -q          |
| -r          |
| -e          |
| -A          |
| -M          |
| -O          |
| --sport     |
| --fwmark    |
| -D          |
| -mtu        |
| -- back     |



# For testing on PlanetLab #

Here are some PlanetLab sites that run Fedora 14 (the most recent version of Fedora Linux currently supported by PlanetLab):
  * planetlabeu-2.tssg.org
  * planetlab2.montefiore.ulg.ac.be
  * onelab10.pl.sophia.inria.fr
  * planetlabeu-1.tssg.org
  * planetlab1.montefiore.ulg.ac.be
  * ops.ii.uam.es
  * utet.ii.uam.es

The most recent version of Modern Traceroute for Linux on these machines is 2.0.18, which is just one minor release behind the most recent version, 2.0.19. It is missing DCCP support.