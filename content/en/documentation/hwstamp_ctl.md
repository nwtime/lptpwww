---
title: "hwstamp_ctl(8): set time stamping policy at the driver level"
description: "Linux PTP man page to set time stamping policy at the driver level."
date: 2014-06-04
---

### hwstamp_ctl(8): set time stamping policy at the driver level

#### SYNOPSIS

<code>hwstamp_ctl -i _interface_ [ -r _rx-filter_ ] [ -t _tx-type_ ]</code>

#### DESCRIPTION

`hwstamp_ctl` is a program used to set and get the hardware time stamping policy at the network driver level with the `SIOCSHWTSTAMP` ioctl(2). The `tx-type` and `rx-filter` values are hints to the driver what it is expected to do. If the requested fine-grained filtering for incoming packets is not supported, the driver may time stamp more than just the requested types of packets.

If neither `tx-type` nor `rx-filter` values are passed to the program, it will use the `SIOCGHWTSTAMP` ioctl(2) to non-destructively read the current hardware time stamping policy.

This program is a debugging tool. The [ptp4l(8)](/documentation/ptp4l/) program does not need this program to function, it will set the policy automatically as appropriate.

#### OPTIONS

<code>**-i _interface_**</code>

: Specify the network interface of which the policy should be changed.

<code>**-r _rx-filter_**</code>

: Specify which types of incoming packets should be time stamped, `rx-filter` is an integer value.

<code>**-t _tx-type_**</code>

: Enable or disable hardware time stamping for outgoing packets, `tx-type` is an integer value.

<code>**-h**</code>

: Display a help message and list of possible values for `rx-filter` and `tx-type`.

<code>**-v**</code>

: Prints the software version and exits.

#### SEE ALSO

ioctl(2), [ptp4l(8)](/documentation/ptp4l/)