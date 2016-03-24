

# Introduction #

This page presents the current status of the new implementation of paris-traceroute and MDA built above libparistraceroute (developed in C). We list the missing features of the both tools and how do we will prior the future developments to improve them.

# New implementation of paris-traceroute and MDA (C) #

In this section we refer to the version of paris-traceroute, mda, and libparistraceroute provided in the "master" [git](Git.md) branch.

## Status ##

Currently, **paris-traceroute and MDA are working** but have some limitations:
  * The both tools use raw socket so _require root privileges_,
  * There are _memory leaks_ in libparistraceroute and thus in paris-traceroute,
  * _Some options of traceroute are not supported_ by paris-traceroute nor MDA (see [roadmap](ParisTraceroutePhase2.md) to see which options are currently supported),

You can use the version of paris-traceroute and libparistraceroute provided in the git master branch.

## Future developments ##

We will prioritize our developments in the following order.
  1. _(functionality)_ Adapt the way [probe tagging](Checksum.md) is performed and improve the socketpool/sniffer to allow normal users to run paris-traceroute and MDA.
  1. _(improvement)_ Improve paris-traceroute overall performance by avoiding to wait for a reply before sending the next probe packet.
  1. _(functionality)_ In long term, support the last options of standard traceroute in paris-traceroute (see [phase 4](ParisTraceroutePhase4.md)).

You could have a larger overview of the future developments of libparistraceroute [here](FutureFeatures.md).

# Regarding the old implementation of paris-traceroute (C++) #

Note that this implementation of paris-traceroute and MDA **is not supported anymore**, because it was mostly a proof of concept. The code is [not well-designed, not efficient and probably buggy](http://devopsreactions.tumblr.com/post/51389786041/looking-at-paris-traceroutes-code). In brief, **you should avoid to use this implementation**.

If you want to test some modification or improvement of paris-traceroute, you are strongly encouraged to develop [using the C version provided in the "master" git branch](Installation.md).

However, you can still retrieve the C++ version of paris-traceroute [here](http://paris-traceroute.net/download).