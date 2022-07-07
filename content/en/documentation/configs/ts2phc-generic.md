---
title: Generic ts2phc Example
description: "Example Linux PTP configuration which uses a PPS signal from a GPS receiver as an input to the SDP0 pin of an Intel i210 card."
date: 2019-07-07
---

### Generic ts2phc Example

This example uses a PPS signal from a GPS receiver as an input to the SDP0 pin of an Intel i210 card.  The pulse from the receiver has a width of 100 milliseconds.

> **Important!**  The polarity is set to `both` because the i210 always time stamps both the rising and the falling edges of the input signal.

Refer to [ts2phc(8)](/documentation/ts2phc/) for more information about the `ts2phc` options.

<pre>
[global]
use_syslog		0
verbose			1
logging_level		6
ts2phc.pulsewidth	100000000
[eth6]
ts2phc.channel		0
ts2phc.extts_polarity	both
ts2phc.pin_index	0
</pre>