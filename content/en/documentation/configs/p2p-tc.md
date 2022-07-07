---
title: Peer to Peer Transparent Clock
description: "Example Linux PTP configuration for Peer to Peer Transparent Clock."
date: 2018-05-25
---

### Peer to Peer Transparent Clock

Peer to Peer Transparent Clock example configuration containing those attributes which differ from the [default configuration](/documentation/configs/default/). Refer to [ptp4l(8)](/documentation/ptp4l/) for the complete list of available options.

<pre>
[global]
priority1		254
free_running		1
freq_est_interval	3
tc_spanning_tree	1
summary_interval	1
clock_type		P2P_TC
network_transport	L2
delay_mechanism		P2P
</pre>