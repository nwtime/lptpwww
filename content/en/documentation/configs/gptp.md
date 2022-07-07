---
title: gPTP
description: "Example Linux PTP 802.1AS configuration for time synchronization over a virtual bridged local area network."
date: 2018-07-31
---

### gPTP

802.1AS example configuration containing those attributes which differ from the [default configuration](/documentation/configs/default/).  Refer to [ptp4l(8)](/documentation/ptp4l/) for the complete list of available options.

<pre>
[global]
gmCapable		1
priority1		248
priority2		248
logAnnounceInterval	0
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
</pre>