---
title: "Installation"
description: "The make command is used to install the Linux PTP source code."
---

### Installation

1. Just type `make`.

2. If you compiled your own kernel (and the headers are not
   installed into the system path), then you should set the
   `KBUILD_OUTPUT` environment variable.

3. In order to install the programs and man pages into `/usr/local`,
   run the `make install` target. You can change the installation
   directories by setttings the variables `prefix`, `sbindir`, `mandir`,
   and `man8dir` on the `make` command line.