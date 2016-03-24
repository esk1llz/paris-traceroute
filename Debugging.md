

# Overview #

Since the paris-traceroute utility depends on the libparistraceroute library which might not be installed during the build, libtool creates a wrapper script (look at the content of paris-traceroute/paris-traceroute).  It is thus not possible to debug this file directly.

# Using gdb and valgrind #

  * Normal run

```
cd your_libparistraceroute_directory
make
./paris-traceroute/paris-traceroute
```

  * Run with gdb and valgrind

```
cd your_libparistraceroute_directory
make
libtool --mode=execute gdb ./paris-traceroute/paris-traceroute
libtool --mode=execute valgrind ./paris-traceroute/paris-traceroute
```

# Questions related to gdb #
## Breakpoints ##

**Problem:**

You'd like to place breakpoint inside the library.

**Solution:**

Suppose that we want to debug paris-traceroute :

```
libtool --mode=execute gdb ./paris-traceroute/paris-traceroute
```

Now suppose that you want to place a breakpoint on a function coded inside the library (let say "mda`_`handler`_`reply"):

```
(gdb) b mda_handler_reply
Function "mda_handler_reply" not defined.
Make breakpoint pending on future shared library load? (y or [n])
```

It means that the symbols of libparistraceroute are not yet loaded. So currently, so this function is not yet known by gdb.

  1. Answer "y".
  1. Run normally the program, and you should reach the breakpoint.

```
(gdb) r -a mda -n 1.1.1.1
[...]
Breakpoint 1, mda_handler (loop=0x605f50, event=0x607a40, pdata=0x605540, 
    skel=0x605590, opts=0x7fffffffdf80) at algorithms/mda.c:727
727                 mda_handler_reply(loop, event, data, skel, options);
```

**Alternative solution:**

  1. Place a breakpoint on the "main" function
  1. Run the program (r ...)
  1. Once the breakpoint is reached, the symbols provided by the library are loaded. You can now place your breakpoint (b ...) on any function of the library and continue (c).

## "Skipped" instructions and "optimized out" ##

**Problem(s):**

  * Some instructions seems to be skipped,
  * When displaying a variable, gdb prints an "optimized out" message

**Solution:**

This is probably due to the "-O2" flag passed to gcc.

  1. Remove this CFLAGS from the appropriate Makefile(s) (e.g libparistraceroute/Makefile, libparistraceroute/paris-traceroute/Makefile)
  1. Recompile the project, and re-run gdb

```
make clean all install
```