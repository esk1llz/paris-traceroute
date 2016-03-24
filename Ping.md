# Options and status #

Lists the options of the ping tool (based on Linux ping).
For the moment we will only focus on the main options that should be easily managed by libparistraceroute.

| **option** | **status**|
|:-----------|:----------|
| [-4](#-4.md) | **implemented** |
| [-6](#-6.md) | **implemented** |
| [-c](#-c.md) count | **implemented** |
| [-D](#-D.md) | **implemented** |
| [-F](#-F.md) Flow\_Label | to be implemented |
| [-i](#-i.md) interval | **implemented but not working properly due to bugs in the lib** |
| [-I](#-I.md) interface\_address | to be implemented |
| [-l](#-l.md) preload | to be implemented? |
| [-n](#-n.md) | **implemented** |
| [-p](#-p.md) pattern | to be implemented? |
| [-Q](#-Q.md) tos | to be implemented? |
| [-q](#-q.md) | **implemented** |
| [-s](#-s.md) packetsize | **implemented** |
| [-t](#-t.md) ttl | **implemented** |
| [-v](#-v.md) | **implemented** |
| [-V](#-V.md) | **implemented** |
| [-w](#-w.md) deadline | to be implemented? |
| [-W](#-W.md) timeout | **implemented** |
| -------    |
| [-P](#-P.md) port | **implemented** |
| [-S](#-S.md) port | **implemented** |
| [-T](#-T.md) | **implemented but not working properly due to bugs in the lib** |
| [-U](#-U.md) | **implemented** |
| [-k](#-k.md) | **implemented but not working properly due to bugs in the lib** |

# Outputs #

# Discussion of options #

## -4 ##

```
  -4                          Use IPv4
```


## -6 ##

```
  -6                          Use IPv6
```


## -c ##

```
  -c count                    Stop after sending count ECHO_REQUEST packets. With deadline option, ping waits for count ECHO_REPLY packets, until the timeout expires.
```


## -D ##

```
  -D
                              Print timestamp (unix  time + microseconds as in gettimeofday) before each line.
```


## -F ##

```
  -F Flow_label               Allocate and set 20 bit flow label on echo request packets. (Only IPv6).
```


## -i ##

```
  -i interval                 Wait interval seconds between sending each packet. 
                              The default is to wait for one second between each packet normally, or not to wait in flood mode.
```


## -I ##

```
  -I interface_address        Set source address to specified interface address.
```


## -l ##

```
  -l preload                  If preload is specified, ping sends that many packets not waiting for reply.
```


## -L ##

```
  -L                          Suppress loopback of multicast packets. This flag only applies if the ping destination is a multicast address.
```


## -n ##

```
  -n                         Do not resolve IP addresses to their domain names
```


## -p ##

```
  -p pattern                 You may specify up to 16 ''pad'' bytes to fill out the packet you send.   
```


## -Q ##

```
  -Q tos                     Set Quality of Service-related bits in ICMP datagrams. tos can be either decimal or hex number. 
                             Traditionally (RFC1349), these have been interpreted as: 0 for reserved (currently being redefined as congestion control), 1-4 for Type of Service and 5-7 for Precedence. 
                             Possible settings for Type of Service are: minimal cost: 0x02, reliability: 0x04, throughput: 0x08, low delay: 0x10. Multiple TOS bits should not be set simultaneously. 
                             Possible settings for special Precedence range from priority (0x20) to net control (0xe0). 
                             You must be root (CAP_NET_ADMIN capability) to use Critical or higher precedence value. You cannot set bit 0x01 (reserved) unless ECN has been enabled in the kernel. 
                             In RFC2474, these fields has been redefined as 8-bit Differentiated Services (DS), consisting of: bits 0-1 of separate data (ECN will be used, here), and bits 2-7 of Differentiated Services Codepoint (DSCP).  
```


## -q ##

```
  -q                         Quiet output. Nothing is displayed except the summary lines at startup time and when finished.
```


## -s ##

```
  -s packetsize              Specifies the number of data bytes to be sent.
```


## -t ##

```
  -t ttl                     Set the IP Time to Live.
```


## -U ##

```
  -U                         Print full user-to-user latency (the old behaviour).
```


## -v ##

```
  -v                         Verbose output.
```


## -V ##

```
  -V                         Show version and exit.
```


## -w ##

```
  -w deadline                Specify a timeout, in seconds, before ping exits regardless of how many packets have been sent or received. In this case ping does not stop after count packet are sent, it waits either for deadline expire or until count probes are answered or for some error notification from network.
```


## -W ##

```
  -W timeout                Time to wait for a response, in seconds.
```



---



## -P ##

```
  -P                        Set PORT as destination port (default: 33457).
```


## -S ##

```
  -S                        Set PORT as source port (default: 33456)
```


## -T ##

```
  -T                        Use TCP. The destination port is set by default to 80.
```


## -U ##

```
  -U                        Use UDP. The destination port is set by default to 53.
```



## -k ##

```
  -k                        Send a TCP ACK packet. (Works only with TCP)
```
