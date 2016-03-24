# Introduction #

We want to port libparistraceroute to work on Contiki OS. At this time, we do not yet know if hardware constraints (or, more precisely, the limitations of hardware emulators, which may not reflect the latest hardware capabilities) will permit the library to be ported.


# Details #

## Issues to be investigated ##

  * Understand the constraints of the operating system and the differences with respect to Linux
    * The major complexity will come from the fact that we don't have the POSIX system calls in Contiki, especially for network I/O, so we will need to write to a different API.
    * Timers will probably be different in Contiki OS.
  * Understand the limitations of the available emulation software. For instance Cooja emulates the Tmote Sky hardware, which is limited to 48 KB of flash memory and 8 or 10 KB of RAM. This might impose a severe constraint on the deployment of libparistraceroute or even rule it out entirely.
    * It would likely be necessary to remove modules from the version of the library that is compiled for Contiki OS.

## Testing environments ##

  * Cooja
  * [SensLAB](http://www.senslab.info/)