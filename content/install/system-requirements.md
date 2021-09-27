---
title: "System Requirements"
---

### System Requirements

In order to run this software, you need Linux kernel version 3.0 or
newer.  Check whether your network interface supports PTP with the
following command.

`ethtool -T eth0`

This command shows whether a MAC supports hardware or software time
stamping.  The following example output indicates support for
hardware time stamping.

<pre>
Time stamping parameters for eth6:
Capabilities:
        hardware-transmit     (SOF_TIMESTAMPING_TX_HARDWARE)
        software-transmit     (SOF_TIMESTAMPING_TX_SOFTWARE)
        hardware-receive      (SOF_TIMESTAMPING_RX_HARDWARE)
        software-receive      (SOF_TIMESTAMPING_RX_SOFTWARE)
        software-system-clock (SOF_TIMESTAMPING_SOFTWARE)
        hardware-raw-clock    (SOF_TIMESTAMPING_RAW_HARDWARE)
PTP Hardware Clock: 1
Hardware Transmit Timestamp Modes:
        off                   (HWTSTAMP_TX_OFF)
        on                    (HWTSTAMP_TX_ON)
Hardware Receive Filter Modes:
        none                  (HWTSTAMP_FILTER_NONE)
        all                   (HWTSTAMP_FILTER_ALL)
</pre>

The next example shows the case where the MAC only supports software
time stamping.  The `ptp4l` program requires either the `-S` command
line argument or the `time_stamping software` configuration option
when using such interfaces.

<pre>
Time stamping parameters for enp6s0:
Capabilities:
        software-transmit     (SOF_TIMESTAMPING_TX_SOFTWARE)
        software-receive      (SOF_TIMESTAMPING_RX_SOFTWARE)
        software-system-clock (SOF_TIMESTAMPING_SOFTWARE)
PTP Hardware Clock: none
Hardware Transmit Timestamp Modes: none
Hardware Receive Filter Modes: none
</pre>

Note the `software-transmit (SOF_TIMESTAMPING_TX_SOFTWARE)`
capability.  If this is lacking, then the MAC cannot be used at
all.  However, adding this capability entails adding a single line
of code to the device driver.