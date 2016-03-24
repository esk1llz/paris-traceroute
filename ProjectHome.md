# Welcome to the Paris Traceroute code development site #

<table cellpadding='10' width='840' border='1'><tr><td>
<b>October 2014 update:</b> New packages of paris-traceroute/MDA and libparistraceroute are now available.</td></tr></table>

## About Paris Traceroute ##

Paris Traceroute is a fundamental upgrade to the standard Traceroute tool that exists on all of the major operating systems. Standard Traceroute was not designed to take into account the presence of load balancing routers, which are widely deployed in today’s Internet, both at the core and at its edges. As a result, standard Traceroute is not aware that there are often multiple paths between a source and a destination in the Internet: the paths will split at a load balancing router at one point along the route and then reconverge some hops later. Standard traceroute is incapable of furnishing this information to its users and instead reports what it states to be a single path but is instead a confusing mixture of pieces of multiple paths. Paris Traceroute is aware of the multiple paths and can report on any single one of them accurately, as well as on all of them.

We invite you to visit the Paris Traceroute [public website](http://www.paris-traceroute.net/) for more information on Paris Traceroute, including academic papers that describe the principles behind the tool and that describe network measurements that reveal the extensive presence of multipath routing in the Internet.

## Download the command-line Paris Traceroute tool ##

There is two existing implementation of Paris Traceroute:
  1. the new implementation written in C and based on libparistraceroute (for further details, see its [current status](ParisTracerouteMdaFutureFeatures.md) and the [installation steps](Installation.md)),
  1. the old implementation written in C++ available here: [bin](http://paris-traceroute.net/) [src](https://code.google.com/p/paris-traceroute/source/checkout?repo=paris-traceroute-old).

You are strongly encouraged to use the C implementation if its status fits your needs since the C++ one is not supported anymore.

## Join the Paris Traceroute development team ##

Many network measurement projects are interested in integrating Paris Traceroute's capabilities into their tools. To make this possible, we have taken the monolithic command-line version of Paris Traceroute, written in C++, and refactored it into a highly modular library, libparistraceroute, written in C. At the time of this writing (April 2013), the library is functional but many features remain to be added.

We welcome people who are enthusiastic about free, open source code development in general and network measurements in particular.

**For more information, and to join us**:
  * Read our [overview of libparistraceroute](Overview.md)
  * Visit our [ideas page](Ideas.md), with ideas for development
  * Write to us through the [Paris Traceroute Google Group](https://groups.google.com/forum/?fromgroups#!forum/paris-traceroute) ([paris-traceroute@googlegroups.com](mailto:paris-traceroute@googlegroups.com))
  * Chat with us on the #paris-traceroute IRC channel (irc.freenode.org)