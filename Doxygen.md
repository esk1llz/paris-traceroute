

# Introduction #

`doxygen` allows to build the documentation according to the Doxyfile provided in the libparistraceroute source directory.

You can rebuild or update this file for instance by using `doxywizard` (package `doxygen-gui` under Debian). The current Doxyfile build the HTML documentation and some graphs using graphviz.

# Installation #

Under Debian, run as root:

```
aptitude update
aptitude install graphviz doxygen
```

# Build the documentation #

Go into the libparistraceroute source directory and run `doxygen`:

```
cd ~/git/paris-traceroute.libparistraceroute/libparistraceroute
doxygen
```

The documentation is generated in `~/git/paris-traceroute.libparistraceroute/libparistraceroute/doc/`.

# Browse the documentation #

Go into the documentation directory and run your favorite web browser (for instance `chromium`):
```
cd ~/git/paris-traceroute.libparistraceroute/libparistraceroute/doc/html
chromium index.html
```

# Customize the documentation build #

This can be useful if you want to for instance LaTeX output, remove the graphviz dependancy, etc.

1. Install doxywizard. For instance, under Debian, run as root:
```
aptitude update
aptitude doxygen-gui
```

2. Update the `Doxyfile` using `doxywizard`:
```
cd ~/git/paris-traceroute.libparistraceroute/libparistraceroute
doxywizard Doxyfile
```

3. Rebuild the documentation:
```
doxygen
```