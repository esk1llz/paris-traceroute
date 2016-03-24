# Paris Traceroute command line options after Phase 2 #

After finishing Phase 2, Paris Traceroute will support the most important options to be backward compatible with the C++ version of Paris Traceroute and Modern Traceroute for Linux.


# Options and status #

The following table lists each command line option of paris-traceroute will support after phase 2 and the current status of development.

The "from" column indicates if the option is from [Modern Traceroute for Linux](BackwardCompatibilityModernInput.md) (M), [BSD Traceroute](BackwardCompatibilityBSDInput.md) (B), [the C++ version of Paris Traceroute](BackwardCompatibilityCppInput.md) (P), or some combination of these, or is a new (N) option. An asterisk (`*`) indicates that there are further considerations (for instance, the option has been modified in some way) that are addressed in [the option discussion](ParisTraceroutePhase2#Discussion_of_options.md), below.

| **option** | **from** | **status**|
|:-----------|:---------|:----------|
| [-4](#-4.md) | M        | implemented |
| [-M](#-M.md) bound,max\_branch | N`*`     | under development |
| [-f](#-f.md) first\_ttl  --first=first\_ttl | MBP      | implemented |
| [-m](#-m.md) max\_ttl  --max-hops=max\_ttl | MBP      |  implemented |
| [-n](#-n.md) | MBP      | implemented |
| [-w](#-w.md) waittime  --wait=waittime | MB`*`    | implemented |
| [-P](#-P.md) prot  --protocol=prot | MB`*`    | implemented |
| [-U](#-U.md)  --udp| M        | implemented |
| [-V](#-V.md)  --version | MP       | implemented |
| [-h](#-h.md) --help| MP       | implemented |


# Discussion of options #

## -4 ##

```
  -4                          Use IPv4
```

We only support IPv4 tracerouting in Phase 2. IPv6 tracerouting is to be added in a later phase.

## -M ##
```
  -M bound,max_branch         Multipath tracing
                              bound: an upper bound on the probability that multipath tracing
                                     will fail to find all of the paths (default 0.05)
                              max_branch: the maximum number of branching points that can
                                     be encountered for the bound still to hold (default 5)
```

**Note:** This command line option breaks backward compatibility with Modern Traceroute's -M option, which indicates the module to use in the traceroute operation. The notion of modules will not be supported by Paris Traceroute.

**Note:** This command line option replaces the "-a MDA" option used by the C++ version of Paris Traceroute.

## -f ##

```
  -f first_ttl  --first=first_ttl
                              Start from the first_ttl hop (instead from 1)
```

This is a very standard option that we will support in Phase 2.

## -m ##

```
  -m max_ttl  --max-hops=max_ttl
                              Set the max number of hops (max TTL to be
                              reached). Default is 30
```

This is a very standard option that we will support in Phase 2.

## -n ##

```
  -n                          Do not resolve IP addresses to their domain names
```

This is a very standard option that we will support in Phase 2. We also will implement the default behavior, which is resolution of IP addresses, in Phase 2.

## -w ##

```
  -w waittime  --wait=waittime
                              Set the number of seconds to wait for response to
                              a probe (default is 5.0). Non-integer (float
                              point) values allowed too
```

**Note:** This command line option is the same as the -T option of the C++ version of Paris Traceroute. We support this as a standard option in Phase 2, but under the name -w, which is what this option is called by both Modern Traceroute for Linux and BSD Traceroute.

## -P ##

```
  -P prot  --protocol=prot    Use raw packet of protocol prot for tracerouting
```

In Phase 2, we will only support UDP tracerouting, with support for other protocols to be added in later phases.

As of Phase 1, we always use raw packets, so this option will be supported in Phase 2. On the other hand, we do not yet support the default behavior, which is to not use raw packets. We need to study the possibility of supporting the default behavior, with an aim to provide this for Phase 4.


## -U ##

```
  -U  --udp                   Use UDP to particular port for tracerouting
                              (instead of increasing the port per each probe),
                              default port is 53
```

In Phase 2, we will only support UDP tracerouting, with support for other protocols to be added in later phases.

## -V ##

```
  -V  --version               Print version info and exit
```

This is a very standard option that we will support in Phase 2.

## -h ##

```
  -h   --help                Read this help and exit
```

This is a very standard option that we will support in Phase 2.