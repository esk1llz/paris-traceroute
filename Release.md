# Introduction #

This page collects some notes about the release process we are adopting for future versions of libparistraceroute.


# Steps #

  * Bump the version number:
    1. in configure.ac
    1. in the .spec files in packages/rpm/SPECS/

  * Tag the release version
```
git tag v0.9
git push --tags
```

  * Build packages (see the appropriate section for more details)

Currently only rpm packages are fully supported and tested:
```
make rpm
```