

# Installation #

## With packages ##

You can install libparistraceroute and several binaries based on libparistraceroute (including paris-traceroute) thanks to dedicated packages. There are two packages listed in those repositories.
  * libparistraceroute is just the libparistraceroute library.
  * paris-traceroute contains both the library and the paris-traceroute binary that uses it.


### rpm-based distributions (Fedora, `RedHat`, Centos, etc.) ###

We provide packages for Fedora 18/19/20 for both i386/i686 and x86\_64 architectures. Due to the limited number of dependencies of libparistraceroute, these rpm should work on most rpm-based distributions, although we have not tested them yet.

You can install the libparistraceroute repository by downloading our .repo file and storing it in /etc/yum.repos.d/, then proceed to the package installation. Note that packages are not yet signed at the moment, and the yum command requires the use of the --nogpgcheck option.

```
wget http://paris-traceroute.net/downloads/packages/Fedora/libparistraceroute.repo -O /etc/yum.repos.d/libparistraceroute.repo
yum install paris-traceroute --nogpgcheck
```

Alternatively, you can download both packages for your architecture and run the following command (eg for x86\_64):

```
rpm -i libparistraceroute-0.9-1.fc20.x86_64.rpm
rpm -i paristraceroute-0.9-1.fc20.x86_64.rpm
```

We will soon release signed packages as well as packages for the paris-ping utility.

### deb-based distributions (Debian, Ubuntu, etc.) ###

Debian-based distributions (Debian 6+, `*`Ubuntu 11.04+ at least) should likewise be up shortly.
[Download packages here](http://www.paris-traceroute.net/downloads/packages)

<a href='Hidden comment: 
=== Live Packages ===

These packages pull the latest HEAD from git and use that to build a package
* Arch Linux    -  [http://www.uea.ac.uk/~xqq07jnu/PKGBUILD PKGBUILD]
* Gentoo Linux  -  [http://www.uea.ac.uk/~xqq07jnu/paris-traceroute-9999.ebuild paris-traceroute-9999.ebuild]
'></a>

### Older packages (obsolete) ###

Old versions of these packages, built from a snapshot taken on June 6th 2012, are still available for various distributions (mostly with .repo files):
  * [CentOS 6](http://download.opensuse.org/repositories/home:/joolesc/CentOS_CentOS-6/)
  * [Fedora 16](http://download.opensuse.org/repositories/home:/joolesc/Fedora_16/)
  * [Fedora 17](http://download.opensuse.org/repositories/home:/joolesc/Fedora_17/)
  * [Mandriva 2010.1](http://download.opensuse.org/repositories/home:/joolesc/Mandriva_2010.1/)
  * [Mandriva 2011](http://download.opensuse.org/repositories/home:/joolesc/Mandriva_2011/)
  * [RHEL 6](http://download.opensuse.org/repositories/home:/joolesc/RedHat_RHEL-6/)
  * [OpenSUSE 11.4](http://download.opensuse.org/repositories/home:/joolesc/openSUSE_11.4/)
  * [OpenSUSE 12.1](http://download.opensuse.org/repositories/home:/joolesc/openSUSE_12.1/)
  * [SLE 11 SP1](http://download.opensuse.org/repositories/home:/joolesc/SLE_11_SP1/)
  * [SLE 11 SP2](http://download.opensuse.org/repositories/home:/joolesc/SLE_11_SP2/)

## With git ##

1) Install needed packages

  * Debian: run the following commands as root:

```
aptitude update
aptitude safe-upgrade
aptitude install autoconf git make libtool libc6-dev
```

  * Fedora: run the following commands as root:

```
yum update
yum install -y autoconf git make libtool libc6-dev
```

2) Get the sources:

```
mkdir ~/git
cd ~/git
git clone https://code.google.com/p/paris-traceroute.libparistraceroute/
cd paris-traceroute.libparistraceroute/libparistraceroute
```

3) Compile the sources:

```
./autogen.sh
./configure
make
```

4) To install the library, run the following command as root:

```
make install
```



# Test the library #

**Method 1:**

You can now compile several tools using libparistraceroute (traceroute, paris-traceroute). You may for instance compile the paris-traceroute command-line tool:

```
cd paris-traceroute
make all install
```

This program needs to create raw socket and thus requires to be run as root (see method 2 to run the test program as a normal user). You may also install and run [fakeroute](Fakeroute.md).

```
paris-traceroute -n 1.1.1.2
```

**Method 2: (TO FIX)**

Alternatively you can use setpcap as root to allow a normal user to run this program.
  * Debian:

```
aptitude install libcap2-bin
```

Run this command as root :

```
setcap cap_net_raw+ep /usr/local/bin/paris-traceroute
LD_LIBRARY_PATH="/usr/local/lib" paris-traceroute -n 1.1.1.2
```

# Troubleshooting #
## libparistraceroute.so not found ##

**Symptoms:**

libparistraceroute is not found, but is properly installed in /usr/local/lib:
```
# paris-traceroute -n 8.8.8.8
paris-traceroute: error while loading shared libraries: libparistraceroute-1.0.so.1: cannot open shared object file: No such file or directory
```

```
# ldd $(which paris-traceroute)
        linux-vdso.so.1 (0x00007fffbb997000)
        libparistraceroute-1.0.so.1 => not found
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f42075c4000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f420798a000)
```

**Solution 1 (recommanded):**

1) Verify whether /usr/local/lib is present in the ld.so.conf configuration. If ld.so.conf is properly configured, you should have a similar result (if not, fix your ld.so.conf configuration):
```
# grep -nr "/usr/local/lib" /etc/ld.so.conf*
/etc/ld.so.conf.d/libc.conf:2:/usr/local/lib
```

2) Run:

```
ldconfig
```

3) Recompile libparistraceroute and paris-traceroute:

```
cd ~/git/paris-traceroute.libparistraceroute/libparistraceroute
make all install
```

4) The paris-traceroute binary should be now properly linked:

```
# ldd $(which paris-traceroute)
        linux-vdso.so.1 (0x00007fff822f4000)
        libparistraceroute-1.0.so.1 => /usr/local/lib/libparistraceroute-1.0.so.1 (0x00007f1870e8b000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f1870ae2000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f18710ce000)
```

**Solution 2:**

Indicate the folder containing libparistraceroute**.so**:
```
LD_LIBRARY_PATH="/usr/local/lib" paris-traceroute -n 1.1.1.2
```