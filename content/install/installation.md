---
title: "Installation"
---

### Installation

1. Just type `make`.

2. If you compiled your own kernel (and the headers are not
   installed into the system path), then you should set the
   <tt>KBUILD_OUTPUT</tt> environment variable.

3. In order to install the programs and man pages into <tt>/usr/local</tt>,
   run the `make install` target. You can change the installation
   directories by setttings the variables <tt>prefix</tt>, <tt>sbindir</tt>, <tt>mandir</tt>,
   and <tt>man8dir</tt> on the `make` command line.