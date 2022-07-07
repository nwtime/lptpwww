---
title: Unicast Master
description: "Example Linux PTP configuration for a unicast master."
date: 2018-04-30
---

### Unicast Master

Unicast Master example configuration containing those attributes which differ from the [default configuration](/documentation/configs/default/).  Refer to [ptp4l(8)](/documentation/ptp4l/) for the complete list of available options.

<pre>
[global]
hybrid_e2e			1
inhibit_multicast_service	1
unicast_listen			1
</pre>