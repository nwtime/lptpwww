---
title: ts2phc Transparent Clock Example
description: "Example Linux PTP configuration which uses ts2phc to form a Transparent Clock."
date: 2019-12-22
---

### ts2phc Transparent Clock Example

This example shows `ts2phc` keeping a group of three Intel i210 cards synchronized to each other in order to form a Transparent Clock. The cards are configured to use their SDP0 pins connected in hardware.  Here `eth3` and `eth4` will be slaved to `eth6`.

> **Important!**  The polarity is set to `both` because the i210 always time stamps both the rising and the falling edges of the input signal.

Refer to [ts2phc(8)](/documentation/ts2phc/) for more information about the `ts2phc` options.

<pre>
[global]
use_syslog		0
verbose			1
logging_level		6
ts2phc.pulsewidth	500000000
[eth6]
ts2phc.channel		0
ts2phc.master		1
ts2phc.pin_index	0
[eth3]
ts2phc.channel		0
ts2phc.extts_polarity	both
ts2phc.pin_index	0
[eth4]
ts2phc.channel		0
ts2phc.extts_polarity	both
ts2phc.pin_index	0
</pre>