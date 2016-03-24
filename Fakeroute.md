

# Introduction #

Fakeroute is a simple tool that simulates network topologies by intercepting traceroute probes towards a set of addresses. Topologies are made of text files describing the successive links, allowing for the presence of load balancers. The tool has been designed to test traceroute-like programs.

# Installation #

Install needed packages
  * Debian: run as root the following commands:

```
aptitude update
aptitude safe-upgrade
aptitude install git python python-setuptools python-netfilter python-nfqueue python-dpkt
```

  * Note: if a python module is not available, no worries, setup.py will install it automatically.

Get the sources:

```
mkdir ~/git
cd ~/git
git clone https://code.google.com/p/paris-traceroute.fakeroute/
```

Then run as root:

```
cd paris-traceroute.fakeroute/fakeroute/
python setup.py install
```

This will install missing python module and configure fakeroute.

# Run fakeroute #

Fkeroute emulate topologies that you can "probe" by performing traceroute measurements. By default, fakeroute propose some samples using the following targets IPs (see [fakeroute/targets](https://code.google.com/p/paris-traceroute/source/browse/?repo=fakeroute#git%2Ffakeroute%2Ftargets)):
  * in IPv4: 1.1.1.1, 1.1.1.2, 1.1.1.3, 1.1.1.4, 1.1.1.5, 1.1.1.6, 1.1.1.7, 1.1.1.8, 1.1.1.9
  * in IPv6: 2001:db8:85a3::8a2e:370:7338

## IPv4 ##

1) If your primary network interface (let say eth0) has no IPv4 address, you cannot route the fakeroute destinations. In our samples every fakeroute nodes belong to 1.1.1.0/24, so you may run as root:

```
ifconfig eth0:1 1.1.1.28/24
```

2) Then run fakeroute as root:

```
cd paris-traceroute.fakeroute/
./fakeroute/fakeroute -4
```

You should see something like this:

```
I: Handling IPv4 probe packets...
   [                          1.1.1.4 ] Weird load balancer
   [                          1.1.1.1 ] Simple asymmetric load balancer
   [                          1.1.1.2 ] Simple load balancer
   [                          1.1.1.3 ] Two-levels simple load balancers
I: Adding iptables rules for handled destinations (see "iptables -nL" and "ip6tables -nL")
I: Running...
```

To simulate random load-balancing, fakeroute uses a random number generator. By default, the generator will be seeded with current linux time. If you would like to define your own seed, add the option [--seed seed].

```
./fakeroute/fakeroute -4 --seed 123
```

3) Run a traceroute measurement toward one of these destinations:

```
paris-traceroute 1.1.1.1
```

4) Press ctrl-c to stop fakeroute.

## IPv6 ##

1) If your primary network interface (let say eth0) has no IPv6 address, you cannot route the fakeroute destinations. In our samples every fakeroute nodes belong to 2001:0db8:85a3:0000:0000:8a2e:0370:7330/64, so you may run as root:

```
ifconfig eth0 inet6 add 2001:0db8:85a3:0000:0000:8a2e:0370:7330/64
```

2) Run fakeroute as root:

```
cd paris-traceroute.fakeroute/
./fakeroute/fakeroute -6
```

You should see something like this:

```
I: Handling IPv6 prove packets...
   [     2001:db8:85a3::8a2e:370:7338 ] IPv6 test
I: Adding iptables rules for handled destinations (see "iptables -nL" and "ip6tables -nL")
I: Running...
```

To simulate random load-balancing, fakeroute uses a random number generator. By default, the generator will be seeded with current linux time. If you would like to define your own seed, add the option [--seed seed].

```
./fakeroute/fakeroute -6 --seed 123
```

3) Run a traceroute measurement toward this destination:

```
traceroute6 2001:db8:85a3::8a2e:370:7338
```

4) Press ctrl-c to stop fakeroute.

# Define your own network topologies #

Fakeroute explores successively the following directories until finding some valid configuration files :
  1. "./targets",
  1. "~/.fakeroute/targets"
  1. "/etc/fakeroute/targets"

Write your own file in one of these directory. The filename must respect the following pattern:

```
<ip_address>-<description>
```

... (for example "1.1.1.1-asymlb"), otherwise this file will be ignored by fakeroute. "ip\_address" denotes a fakeroute destination that you can probe with a traceroute-like tool.

The first line of your file should be:

```
#DESC description of your network topology
```

Then, write one line per arc. An arc involves a source vertex (which may be either "start" or an IP address) and a target vertex (which may be "end" or an IP address) separated by a space character.
  * start stands for the node running traceroute measurements
  * end stands for the target node of the traceroute

**Full example:**

```
# DESC Simple asymmetric load balancer
start 10.0.0.1
10.0.0.1 10.0.0.2
10.0.0.2 10.0.1.3
10.0.0.2 10.0.2.3
10.0.1.3 10.0.1.4
10.0.2.3 10.0.0.5
10.0.1.4 10.0.0.5
10.0.0.5 10.0.0.6
10.0.0.6 10.0.0.7
10.0.0.7 end
```

You may use "plot" (see next section) to draw the corresponding network topology into a png file.

# Drawing fakeroute topologies #

1) Install needed packages (assuming fakeroute already works fine)
  * Debian: run as root the following commands

```
aptitude update
aptitude safe-upgrade
aptitude install python-pydot
```

2) Go to the fakeroute directory and run plot by passing as parameter the fakeroute target IP (for instance 1.1.1.1):

```
cd paris-traceroute.fakeroute/
./fakeroute/plot 1.1.1.1
```

3) You should see the following output:

```
I: Converting './targets/1.1.1.1-asymlb' into '1.1.1.1-asymlb.png'
```

# Fakeroute and wireshark #

Thanks to wireshark, you can observe packets returned by fakeroute by sniffing "lo" interface.