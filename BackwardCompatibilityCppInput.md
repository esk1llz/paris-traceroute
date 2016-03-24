# Backward compatibility of input with the old C++ version of Paris Traceroute #

In the absence of an extensive user community for the C++ version of Paris Traceroute, we can afford to not maintain strict backward compatibility with this version. However, in order to retire the C++ version, we need for the new library-based version of Paris Traceroute to replicate the older version's essential functionalities.

Here, we review the C++ version's command line arguments and state what our objectives are for replicating them. In any case where there is a possible conflict with a classic traceroute command line argument, classic traceroute takes precedence, and we need to consider whether to rename or to discard the C++ Paris Traceroute argument.

# Objectives and status #

The following table lists each command line option of the C++ version of Paris Traceroute, our objectives with regards to the option in the new library-based version of Paris Traceroute, and the current status of development. There is a link from each option to a detailed discussion of that option.

| **option** | **relation to Modern Traceroute for Linux and to BSD Traceroute** | **objectives** | **status** |
|:-----------|:------------------------------------------------------------------|:---------------|:-----------|
| [-h](#-h.md) | Paris Traceroute only                                             | later          | to be implemented |
| [-V](#-V.md) | also a Modern Traceroute option                                   | later          | to be implemented |
| [-v](#-v.md) | Paris Traceroute option; may correspond to BSD Traceroute's -v    | later          | to be implemented |
| [-Q](#-Q.md) | Paris Traceroute only                                             | later          | to be implemented |
| [-f](#-f.md) | also a Modern Traceroute and BSD Traceroute option                | immediately    | to be implemented |
| [-m](#-m.md) | also a Modern Traceroute and BSD Traceroute option                | immediately    | to be implemented |
| [-p](#-p.md) | corresponds to Modern Traceroute's and BSD Traceroute's -P option; different from their -p option | immediately    | to be implemented |
| [-s](#-s.md) | Paris Traceroute only; different from Modern Traceroute's and BSD Traceroute's -s option | later          | to be implemented |
| [-d](#-d.md) | Paris Traceroute only; different from Modern Traceroute's and BSD Traceroute's -d option | later          | to be implemented |
| [-t](#-t.md) | also a Modern Traceroute and BSD Traceroute option                | later          | to be implemented |
| [-w](#-w.md) | corresponds to Modern Traceroute's and BSD Traceroute's -z option; different from their -w option | later          | to be implemented |
| [-T](#-T.md) | corresponds to Modern Traceroute's and BSD Traceroute's -w option; different from their -T option; | immediately    | to be implemented |
| [-q](#-q.md) | also a Modern Traceroute and BSD Traceroute option                | later          | to be implemented |
| [-M](#-M.md) | Paris Traceroute only; different from Modern Traceroute's -M option, which is itself different from BSD Traceroute's -M option | later          | to be implemented |
| [-a](#-a.md) | Paris Traceroute only; different from BSD Traceroute's -a option  | immediately    | to be implemented |
| [-n](#-n.md) | also a Modern Traceroute and BSD Traceroute option                | later          | to be implemented |
| [-i](#-i.md) | Paris Traceroute only; different from Modern Traceroute's and BSD Traceroute's -i option | later          | to be implemented |
| [-l](#-l.md) | related to, but not identical to, Modern Traceroute's --back option; different from Modern Traceroute's -l option  | later          | to be implemented |
| [-F](#-F.md) | Paris Traceroute only; different from Modern Traceroute's and BSD Traceroute's -F option | drop entirely  | -          |
| [-B](#-B.md) | Paris Traceroute only                                             | later          | to be implemented |
| [-c](#-c.md) | Paris Traceroute only                                             | drop entirely  | -          |
| [-E](#-E.md) | Paris Traceroute only                                             | later          | to be implemented |
| [-r](#-r.md) | Paris Traceroute only; different from Modern Traceroute's and BSD Traceroute's -r option |

# C++ Paris Traceroute at the command line #

```
$ paris-traceroute 
Print the route packets take to network host

Usage: traceroute [Options] [Destination]

Options:
  -h, --help               print this help
  -V, --version            print version
  -v, --verbose            print debug messages
  -Q, --quiet              print only results
  -f, --first_ttl=TTL      set the initial ttl to TTL (default: 1)
  -m, --max_ttl=TTL        set the maximum ttl to TTL (default: 30)
  -p, --protocol=PROTOCOL  use PROTOCOL to send probes (udp, tcp, icmp)
                           The default is 'udp'
  -s, --source_port=PORT   set PORT as source port (default: 33456) pid: use PID
  -d, --dest_port=PORT     set PORT as destination port (default: 33457)
  -t, --tos=TOS            set TOS as type of service (default: 0)
  -w MS                    wait MS ms between each probe (default: 50ms)
  -T, --timeout=MS         set a timeout of MS ms on each probe
                           The default is 5000ms
  -q, --query=NBR          send NBR probes to each host (default: 3)
  -M, --missing_hop=HOP    stop traceroute after HOP consecutive down hops
                           The default is 3
  -a, --algo=ALGO          algorithm to use (--algo=help for more help)
                           The default is 'hopbyhop'
  -L, --length=LEN         set the packet length to be used in outgoing packets
                           The default is 0
  -n                       print hop addresses numerically
                           The default is to print also hostnames
  -i  --ipid               print the IP Identifier of the reply
  -l  --print_ttl          print the TTL of the reply
  -F                       targets file for the MT algo
  -B                       set the bandwidth in packets/s
  -c                       number of threads (default 1)
  -E                       probe multiplier
  -r                       set the return flow identifier
$ paris-traceroute --algo=help
tupleroute - algorithms

  --algo=null              Do nothing.

  --algo=hopbyhop          Send x packets with the same ttl, then wait for all
                           replies or a timeout. Increment the ttl and reiter
                           the operation until we reached the destination.
                           All packets hold the same 5-tuples (addresses, ports
                           and protocol fields).

  --algo=packetbypacket    Send one packet at a time, then wait for a reply or
                           a timeout. Reiter the operation until we reached the
                           destination. All packets are exactly the same except
                           the TTL and checkum fields of the IP header.

  --algo=scout             Send a scout probe with a ttl max to the destination.
                           If the destination can be reached, it computes the
                           number of hops used to reach the destination and
                           start the concurrent algorithm with a max_ttl equal
                           to this number of hops. If the destination cannot be
                           reached, the hopbyhop algorithm will be used instead.
                           This algorithm is only usable with udp probes

  --algo=concurrent        Send all probes from min_ttl to max_ttl and then wait
                           for all replies or a timeout. All packets hold the
                           same 5-tuples.

  --algo=exhaustive        Tries to classify load balancing
                           and find all the interfaces for each hop.
$
```

## Manual page ##

```
PARIS-TRACEROUTE(8)                                        PARIS-TRACEROUTE(8)



NAME
       paris-traceroute  -  print  the  IP-level  routes  between two Internet
       hosts.

SYNOPSIS
       paris-traceroute [ -fhilnqvV ] [ -b initial_id ]
               [ -d dest_port ] [ -a algorithm ] [ -f first_ttl ]
               [ -L packetlen ] [ -m max_ttl ]
               [ -M max_missing_hops ] [ -p protocol ]
               [ -q nqueries ] [ -s source_port ] [ -t tos ]
               [ -T delaymsecs ] [ -w waittime ]
               host

DESCRIPTION
       Paris traceroute is a new version of the well-known  network  diagnosis
       tool.   It addresses problems caused by load balancers with the initial
       traceroute(8) implementation. By controlling the flow identifier of the
       probes,  it is able to follow accurate paths in networks with load bal-
       ancers.  It is also able to find all the load  balanced  paths  to  the
       destination.    Finally,   it  enriches  its  output  with  information
       extracted from the received packets, allowing a more  precise  analysis
       of the discovered paths.

       Options are:

       -a     Set the probing algorithm:

       hopbyhop
              Send  q  (configured with the -q flag) probes with the same TTL,
              then wait for all the replies or a timeout.  Increment  the  TTL
              and  reiter  the  operation until we reach the destination.  All
              packets hold the same flow identifier.

       packetbypacket
              It is the classic traceroute(8) algorithm: send one probe  at  a
              time,  then  wait for a reply or a timeout. Reiter the operation
              until we reach the destination.

       concurrent
              Send all the probes from min_ttl to max_ttl  and  wait  for  all
              replies or a timeout.

       scout  Send  a  scout  probe with a ttl max to the destination.  If the
              destination can be reached, compute the number of hops  used  to
              reach  the destination and start the concurrent algorithm with a
              max_ttl equal to this number of hops. If the destination  cannot
              be  reached,  the hopbyhop algorithm will be used instead.  This
              algorithm is only usable with UDP probes.

       exhaustive
              Print all the possible "load balanced" paths to the destination.
              (See section EXHAUSTIVE ALGORITHM )

       -b     Set the initial probe identifier.

       -d     Set the the UDP/TCP destination port (default: 33457).

       -f     Set the initial ttl (default: 1).

       -h     Print help.

       -i     Print  the "IP Identifier" value of the responses. It is used to
              identify the different interfaces of a router,  or  uncover  NAT
              boxes.

       -l     Display  the  ttl value of the reply. Useful to study asymmetric
              routing and NAT boxes.

       -L     Set the data length to be used in  outgoing  packets.  (default:
              0).

       -m     Set the maximum ttl (default: 30).

       -M     Set  the  maximum  number of consecutive unresponsive hops which
              causes the program to abort (default 3).

       -n     Print hop addresses numerically. The default is  to  print  also
              hostnames.

       -p     Set the protocol to use (possible values: udp, tcp, icmp).

       -q     Set the number of probes per hop (default: 3).

       -s     Set the UDP/TCP source port (default: 33456).

       -t     Set  the  Type of Service (default: 0). This field is taken into
              account by many per-flow load balancers: in presence of  such  a
              load balancer, packets having different TOS values are likely to
              follow a different paths.

       -T     Set the time to wait between probes,  in  milliseconds  (default
              50ms).

       -v     Print debug messages.

       -V     Print the program version.

       -w     Set  the  time  to wait for a response, in milliseconds (default
              5000ms).


EXHAUSTIVE ALGORITHM
       With the deployment of load balancing, there is no longer only one path
       between two Internet hosts.  This algorithm sends enough probes at each
       hop to find all the possible interfaces. Unlike the  other  algorithms,
       it  varies the flow identifier of the probes in a controlled manner, to
       ensure the discovery of all  the  interfaces  with  a  high  confidence
       degree.   It  also  categorizes load balancers as "per-packet" (pseudo-
       random, round-robin packet balancing) or "per-flow" (packets  belonging
       to the same flow follow the same path).

       In case of per-flow load balancing, it prints additional information to
       track flows.  The following trace shows the enriched output:

              14  1.1.1.1:0,1,3  539.065 ms  1.1.1.2:2,4,5  492.152 ms
              15  2.2.2.2:0,1,3  563.163 ms  2.2.2.3:2,4,5  470.919 ms

       Integers listed after the interface addresses are  "flow  identifiers":
       they  are used to identify a flow in the set of interfaces found by the
       algorithm. For  example,  flow  #0  traverses  interfaces  1.1.1.1  and
       2.2.2.2. This is the same for flows 1 and 3 while flows 2, 4 and 5 tra-
       verse 1.1.1.2 and 2.2.2.3.


OUTPUT
       The following information are extracted from the response  packets  and
       displayed:


       Response TTL
              The  TTL of the responses (from the routers and the destination)
              is optionally displayed in square brackets (Use the -l flag ).

       Original TTL
              This is the TTL of the probe when it was received and dropped by
              the router.  If the original TTL is different than 1, it is dis-
              played with a !Tx symbol, where x is the value of the TTL.   For
              example,  !T0 indicates that the value of the TTL was 0 when the
              probe reached the router that discarded it.

       IP Identifier
              This the identifier of the IP error packet sent by  the  router.
              This  field  is set with the value of an internal 16-bit counter
              usually incremented for each packet sent. This value is  option-
              ally  displayed  inside  brackets. For instance {1234} indicates
              that the probe had its identifier set to 1234.

       MPLS labels
              If the packet contains ICMP extensions for MPLS, the MPLS  label
              stack  is diplayed in an additionnal line just after the current
              hop line.  Labels of the same stack are  separated  with  a  "|"
              character.

       Other ICMP error messages
              Paris  traceroutes uses the same convensions as traceroute(8) to
              display  unexpected   ICMP   messages   (i.e.   different   than
              TIME_EXCEEDED, PORT_UNREACHABLE and ECHO_REPLY).

SEE ALSO
       traceroute(8), pathchar(8), netstat(1), ping(8)

AUTHOR
       The  initial  version  of traceroute(8) was implemented by Van Jacobson
       from a suggestion by Steve Deering.  Paris traceroute  was  implemented
       by Xavier Cuvellier. Debugged and enhanced by Brice Augustin.

       The current version is available at:

              http://www.paris-traceroute.net

BUGS
       Please send bug reports to brice.augustin@rp.lip6.fr.



4.3 Berkeley Distribution       28 August 2006             PARIS-TRACEROUTE(8)
```

# Discussion of options #

## -h ##
```
  -h, --help               print this help
```

## -V ##
```
  -V, --version            print version
```
## -v ##
```
  -v, --verbose            print debug messages
```

## -Q ##
```
  -Q, --quiet              print only results
```

## -f ##
```
  -f, --first_ttl=TTL      set the initial ttl to TTL (default: 1)
```

## -m ##
```
  -m, --max_ttl=TTL        set the maximum ttl to TTL (default: 30)
```

## -p ##
```
  -p, --protocol=PROTOCOL  use PROTOCOL to send probes (udp, tcp, icmp)
                           The default is 'udp'
```

## -s ##
```
  -s, --source_port=PORT   set PORT as source port (default: 33456) pid: use PID
```

## -d ##
```
  -d, --dest_port=PORT     set PORT as destination port (default: 33457)
```

## -t ##
```
  -t, --tos=TOS            set TOS as type of service (default: 0)
```

## -w ##
```
  -w MS                    wait MS ms between each probe (default: 50ms)
```

## -T ##
```
  -T, --timeout=MS         set a timeout of MS ms on each probe
                           The default is 5000ms
```

## -q ##
```
  -q, --query=NBR          send NBR probes to each host (default: 3)
```

## -M ##
```
  -M, --missing_hop=HOP    stop traceroute after HOP consecutive down hops
                           The default is 3
```

## -a ##
```
  -a, --algo=ALGO          algorithm to use (--algo=help for more help)
                           The default is 'hopbyhop'
```

## -L ##
```
  -L, --length=LEN         set the packet length to be used in outgoing packets
                           The default is 0
```

## -n ##
```
  -n                       print hop addresses numerically
                           The default is to print also hostnames
```

## -i ##
```
  -i  --ipid               print the IP Identifier of the reply
```

## -l ##
```
  -l  --print_ttl          print the TTL of the reply

```

## -F ##
```
  -F                       targets file for the MT algo
```

## -B ##
```
  -B                       set the bandwidth in packets/s
```

## -c ##
```
  -c                       number of threads (default 1)
```

## -E ##
```
  -E                       probe multiplier
```

## -r ##
```
  -r                       set the return flow identifier
```