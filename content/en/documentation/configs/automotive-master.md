---
title: Automotive Master
description: "Example Linux PTP master automotive profile."
date: 2021-01-18
---

### Automotive Master

Automotive profile example configuration for master containing those attributes which differ from the [default configuration](/documentation/configs/default-cfg/). Refer to [ptp4l(8)](/documentation/ptp4l/) for the complete list of available options.

<pre>
[global]
# Options carried over from gPTP.
gmCapable		1
priority1		248
priority2		248
logSyncInterval		-3
syncReceiptTimeout	3
neighborPropDelayThresh	800
min_neighbor_prop_delay	-20000000
assume_two_step		1
path_trace_enabled	1
follow_up_info		1
transportSpecific	0x1
ptp_dst_mac		01:80:C2:00:00:0E
network_transport	L2
delay_mechanism		P2P
#
# Automotive Profile specific options
#
BMCA			noop
serverOnly		1
inhibit_announce	1
asCapable               true
inhibit_delay_req       1
</pre>