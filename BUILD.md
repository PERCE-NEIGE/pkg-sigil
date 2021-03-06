Build the Self-Contained Sigil Installer for Linux (including all dependencies) from Scratch
============================================================================================

See [installing/removing the unofficial standalone version of Sigil](./INSTALL.md).

The code in this repository is ONLY intended to run on linux.

This repository contains code to automate the process of building Sigil, including all its dependencies, from scratch, for the linux platform.

In general, a build proceeds in two steps, first build all the dependencies, then building Sigil and the installer package itself.

Requirements
---------------

You need to have [docker](https://www.docker.com/) installed and running as the linux builds are done in a docker Ubuntu 14.04 (Trusty) container. Most linux distributions have docker in their standard repositories.

Set the environment variable `SIGIL_SRC_DIR` to point to the location of the sigil source code. For example, if you unzipped the calibre source archive into: `/usr/dev/sigil-src` then set the environment variable as:

```
export SIGIL_SRC_DIR=/usr/dev/sigil-src
```

Building the dependencies
----------------------------

To make the linux sigil installer package, it uses docker. Docker images/containers can take up a lot of space, so be certain you know where your docker images are kept, and that you have plenty of disk-space there. The Sigil images themselves create a tmp filesystem of 6Gb (but is removed after the process runs).

To build the 64bit and 32bit dependencies for Sigil, run:

```
./pkg 64
./pkg 32
```

The output (after a very long time) will be in `build/linux/[32|64]`

Building Sigil and the installer package
------------------------------------------

Now you can build Sigil itself using these dependencies. To do that, run:

```
SIGIL_SRC_DIR=/whatever ./pkg 64 sigil
SIGIL_SRC_DIR=/whatever ./pkg 32 sigil
```

The output will be `build/linux/[32|64]/dist/Sigil-vX.X.X.run.xz` which is the linux
binary installer for sigil. This is a self extracting Makeself archive that will install Sigil to /opt/sigil if the script is run as root, or it will install to ~/opt/sigil if you run it as a normal user. It will require a minimum of (g)libc 2.19 to run (ldd --version).

Credits
---------

A huge debt of gratitude goes out to [Kovid Goyal whose build-calibre repository](https://github.com/kovidgoyal/build-calibre) provided the inspiration (and much of the code itself) for this automated build project.
