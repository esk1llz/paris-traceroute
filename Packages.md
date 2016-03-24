

# Introduction #

In order to make it easy for users to install the library it is important to have packages available for a large number of popular linux distributions. This page provides the step to build deb and rpm packages.

These packages are available on [this page](Installation.md).

# Building Packages #
To make your own packages you need to do the following:

## rpm packages ##

### Prerequisites ###

Install the needed packages
  * Debian: run the following commands as root

```
aptitude update
aptitude safe-upgrade
aptitude install mock rpm
```

### Package creation ###

1) Clone a copy of the repository:

```
git clone https://code.google.com/p/paris-traceroute.libparistraceroute
```

2) Run automake and friends:

```
cd paris-traceroute.libparistraceroute/libparistraceroute
./autogen.sh
./configure
```

3) Run the following command to make source rpms as well as your local architecture package.

```
make rpm
```

Packages will be in ./packages/rpm/RPMS. Source packages will be in ./packages/rpm/SRPMS. Both the libparistraceroute and paris-traceroute packages are built by these commands. For other packages based on libparistraceroute, you may use the .spec file for paris-traceroute as a template and add a line to Makefile.am to build your package as well (remember to re-run the configure script after modifying Makefile.am)


4) We will use the 'mock' tool to create packages for other architectures:

```
mock -r fedora-18-i386 --rebuild packages/rpm/SRPMS/libparistraceroute-0.9-1.src.rpm --resultdir=/tmp
mock -r fedora-18-i386 --rebuild packages/rpm/SRPMS/paris-traceroute-0.9-1.src.rpm --resultdir=/tmp
mock -r fedora-18-x86_64 --rebuild packages/rpm/SRPMS/libparistraceroute-0.9-1.src.rpm --resultdir=/tmp
mock -r fedora-18-x86_64 --rebuild packages/rpm/SRPMS/paris-traceroute-0.9-1.src.rpm --resultdir=/tmp

mock -r fedora-19-i386 --rebuild packages/rpm/SRPMS/libparistraceroute-0.9-1.src.rpm --resultdir=/tmp
mock -r fedora-19-i386 --rebuild packages/rpm/SRPMS/paris-traceroute-0.9-1.src.rpm --resultdir=/tmp
mock -r fedora-19-x86_64 --rebuild packages/rpm/SRPMS/libparistraceroute-0.9-1.src.rpm --resultdir=/tmp
mock -r fedora-19-x86_64 --rebuild packages/rpm/SRPMS/paris-traceroute-0.9-1.src.rpm --resultdir=/tmp

mock -r fedora-20-i386 --rebuild packages/rpm/SRPMS/libparistraceroute-0.9-1.src.rpm --resultdir=/tmp
mock -r fedora-20-i386 --rebuild packages/rpm/SRPMS/paris-traceroute-0.9-1.src.rpm --resultdir=/tmp
mock -r fedora-20-x86_64 --rebuild packages/rpm/SRPMS/libparistraceroute-0.9-1.src.rpm --resultdir=/tmp
mock -r fedora-20-x86_64 --rebuild packages/rpm/SRPMS/paris-traceroute-0.9-1.src.rpm --resultdir=/tmp
```

Useful mock configuration files are available at https://github.com/davidhrbac/mock-configs

NOTE: rpm packages are not signed at the moment.

## deb packages ##

### Prerequisites ###

Install the needed packages by running the following commands as root

```
aptitude update
aptitude safe-upgrade
aptitude install devscripts
```

### Package creation ###

```
make dist
make deb
```

NOTE: At this stage, debian packages are experimental. They are not signed and do not pass lintian tests for instance.

<a href='Hidden comment: 
* If you are a maintainer of paris-traceroute packages, you can produce a signed packet by running:
```
debuild 
```
* Otherwise, run:
```
dpkg-buildpackage -us -uc
```
'></a>