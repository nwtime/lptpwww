---
title: End to End Transparent Clock
description: "Example Linux PTP configuration for an End to End Transparent Clock."
date: 2018-05-25
---

### End to End Transparent Clock

End to End Transparent Clock example configuration containing those attributes which differ from the [default configuration](/documentation/configs/default/).  Refer to [ptp4l(8)](/documentation/ptp4l/) for the complete list of available options.

<pre>
[global]
priority1		254
free_running		1
freq_est_interval	3
tc_spanning_tree	1
summary_interval	1
clock_type		E2E_TC
network_transport	L2
</pre>