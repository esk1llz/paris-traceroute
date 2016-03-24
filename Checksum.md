# Introduction #

## How probe/reply matching is performed? ##

In libparistraceroute we need to map a probe packet with its corresponding reply. This is achieved by assigning to each probe with an unique identifier named **tag**.

In a traceroute-like measurement, the ICMP replies (of type 'destination unreachable' or 'time exceed') carry the beginning of the corresponding probe packet.

Thus, if the tag is encoded in the header of the probe packet, we can easily extract it from the ICMP data. When such a packet is sniffed, libparistraceroute dissects the ICMP data to rebuild the nested protocol layers (see in probe.c function probe\_wrap\_packet).

## Example ##

For example, if traceroute send an IPv4/UDP packet, the corresponding IPv4/ICMP reply will be interpreted as IPv4/ICMP/IPv4/UDP packet (simple principle with IPv6).

In practice, we encode the tag in the checksum of the UDP layer since the most of the other header fields may be specified by the user (target IP, ports, etc.) or cannot be modified (source IP, IP version, etc.). Then we should retrieve the tag by extracting the UDP of the IP/ICMP/IP/UDP reply.

## Problems ##

First, checksums depend on the rest of the packet. Hence, forcing the checksum to a given value also require to update another part of the packet in order to remain it well-formed.

Second, altering the checksum requires raw socket and thus root privileges or net capabilities.

# Current implementation #

For sake of simplicity we consider IPv4/UDP probe packets in this section.

## Idea ##

1) We write an arbitrary tag in the 2 first bytes of the payload
2) Update checksums involved in the packet to get a well-formed packet.
3) Swap the checksum and those two bytes containing the tag.

We show in the next paragraph that the packet remains well-formed. The resulting packet carries the tag in its UDP checksum.

## Proof ##

IPv4 checksum only depends on the IP header contents, so altering the UDP header and the payload do not alter the IPv4 checksum.

UDP checksum depends on its corresponding UDP **[pseudo-header](http://en.wikipedia.org/wiki/User_Datagram_Protocol#Pseudo-Headers)** (which depends on the IP layer and the UDP header except the UDP checksum) and the UDP data. So, let us show that we can swap its checksum and the 2 first byte of the UDP data while remaining the packet consistent.

Look libparistraceroute/protocol.c, which implements the csum function used in the protocol modules (e.g. protocols/{ipv4, udp, icmp, ...}.c) :

```
uint16_t csum(const uint16_t * bytes, size_t size) {
    // Adapted from http://www.netpatch.ru/windows-files/pingscan/raw_ping.c.html
    uint32_t sum = 0;

    while (size > 1) {
        sum += *bytes++;
        size -= sizeof(uint16_t);
    }
    if (size) {
        sum += * (const uint8_t *) bytes;
    }
    sum  = (sum >> 16) + (sum & 0xffff);
    sum += (sum >> 16);
    return (uint16_t) ~sum;
}
```

We can swap any pair of uint16\_t x and y stored in bytes the offset of x and y is a multiple of 2 bytes. Fortunately this is the case of the UDP checksum and the 2 first bytes of the payload.

## Implementation ##
### Fetching bytes used by csum ###

Each protocol module (udp.c, icmp.c, etc.) related to a protocol layer provides a function dedicated to compute its own checksum.

```
bool udp_write_checksum(unsigned char *buf, buffer_t * psh);
```

Note that depending on the protocol, its checksum may involve other layers (e.g. UDP, TCP...). For instance, in [udp.c](https://code.google.com/p/paris-traceroute/source/browse/libparistraceroute/libparistraceroute/protocols/udp.c?repo=libparistraceroute) this following functions retrieve the pseudo header for a IPv4/UDP packet and compute the UDP checksum:

```
buffer_t * udp_create_psh_ipv4(unsigned char *ipv4_buffer);
```

### Swapping 2 first bytes of the payload and the checksum ###

This is performed in [network.c](https://code.google.com/p/paris-traceroute/source/browse/libparistraceroute/libparistraceroute/network.c?repo=libparistraceroute) in function network\_tag\_probe.

1) We encode tag 0x0100 in the payload.
2) We update the checksum:

```
LAYER: ipv4
----------
version         4          (0x4)
ihl             5          (0x5)
tos             0          (0x00)
length          30         (0x001e)
identification  256        (0x0100)
fragoff         0          (0x0000)
ttl             1          (0x01)
protocol        17         (0x11)
checksum        27677      (0x6c1d)
src_ip          132.227.125.240
dst_ip          8.8.8.8

    LAYER: udp
    ----------
    src_port        24001      (0x5dc1)
    dst_port        30000      (0x7530)
    length          10         (0x000a)
    checksum        6405       (0x1905)

        PAYLOAD:
        ----------
        size            2
        data            01 00
```

... which corresponds to the well-formed packet:

```
45 00 00 1e 01 00 00 00 01 11 6c 1d 84 e3 7d f0 08 08 08 08 5d c1 75 30 00 0a 19 05 01 00
```

3) We swap the uint16 related to the UDP checksum and the two (first) bytes of the payload

```
45 00 00 1e 01 00 00 00 01 11 6c 1d 84 e3 7d f0 08 08 08 08 5d c1 75 30 00 0a 01 00 19 05
```

## Drawback of this approach ##

The main drawback is we require RAW socket since we craft the whole packet including the IP and UDP header, and thus root privileges or net  [capabilities](http://www.lindevdoc.org/wiki/Capability).

# Future implementation #

## Idea ##

We can predict what would be the checksum for a given value of the payload.

Thus, we can put an arbitrary payload to get the checksum we desire (and thus encode our probe tag in the checksum). Thus we wouldn't require to alter the UDP header anymore, and thus we don't need raw socket.

As a consequence, we could directly use sockets not requiring root privileges or network capabilities and thus, a user could directly run paris-traceroute.