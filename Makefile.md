

# Adding files in the project #

Here we consider that you want to add file in libparistraceroute, but the principle remains the same with an application.

  1. Edit libparistraceroute/Makefile.am
  1. Add your {.h} file(s) in the "nobase\_libparistraceroute`_`@LIBRARY\_VERSION@`_`la\_HEADERS" variable. Please respect the lexicographic order.
  1. Add your {.c} file(s) in the "libparistraceroute`_`@LIBRARY\_VERSION@`_`la`_`SOURCES" variable. Please also respect the lexicographic order.
  1. Add resources file(s) (.txt, ...) in EXTRA\_DIST variable in ../Makefile.am. Please also respect the lexicographic order.
  1. Regenerate the Makefile

```
./autogen.sh
./configure
make
```

# Adding a new application in the project #

Currently, libparistraceroute sources are split into several directories (one for the lib and one per application). For instance:

  * _libparistraceroute:_ sources related to the library
  * _paris-traceroute:_ sources related to the "paris-traceroute" application
  * _traceroute:_ sources related to the "traceroute" application


  1. For each executable, let say 'foo', based on libparistraceroute you want to add, create a dedicated directory 'foo' at the same level and copy Makefile.am and paris-traceroute.c.

```
mkdir foo
cp paris-traceroute/paris-traceroute.c foo/foo.c
cp paris-traceroute/Makefile.am foo/
```

> 2. Edit Makefile.am and replace every "paris-traceroute" by "foo" :

```
@SET_MAKE@

AUTOMAKE_OPTIONS = foreign

# the program to build (the names of the final binaries)
bin_PROGRAMS = foo

# list of sources for the traceroute binary
foo_SOURCES =    \
                        foo.c

foo_LDADD = -lparistraceroute-1.0
foo_LDFLAGS = -L../libparistraceroute
foo_DEPENDENCIES =
```

> 3. Now, register your application.

> 3.a. Edit configure.ac. You have to add a AC\_CONFIG\_SRCDIRS test. You also have to complete the AC\_CONFIG\_FILES variable. Please respect the lexicographic order.

```
...

# Check if the source folder is available
AC_CONFIG_SRCDIR([foo/foo.c])
AC_CONFIG_SRCDIR([paris-traceroute/paris-traceroute.c])
AC_CONFIG_SRCDIR([traceroute/traceroute.c])

...

AC_CONFIG_FILES(
...
    [foo/Makefile]
    [paris-traceroute/Makefile]
    [traceroute/Makefile]
...
)

...
```

> 3.b. Optionnally, you can also add your application in the Makefile.am file stored in the libparistraceroute root directory. Hence your application will be automatically (re)built whenever the library is (re)compiled.

```
@SET_MAKE@

AUTOMAKE_OPTIONS = foreign
ACLOCAL_AMFLAGS = -I m4

# The subdirectories of the project to go into
SUBDIRS = libparistraceroute foo paris-traceroute traceroute man doc
```


> 4. Refresh/build the makefiles:

```
./autogen
./configure
```

> 5. Compile your program.

```
cd foo
make
```