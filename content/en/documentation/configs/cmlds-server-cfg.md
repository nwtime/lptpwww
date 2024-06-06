---
title: CMLDS Server Configuration
description: "Example Linux PTP Common Mean Link Delay Service (CMLDS) example configuration for a CMLDS server, containing those attributes which differ from the defaults."
date: 2024-02-20
---

#### cmlds-server.cfg

Common Mean Link Delay Service (CMLDS) example configuration for a CMLDS Link Port, containing those attributes which differ from the defaults.  See the file, [default.cfg](/documentation/configs/cmlds-server-cfg/), for the complete list of available options.

<pre>
[global]
clientOnly			1
clockIdentity			C37D50.0000.000000
free_running			1
ignore_transport_specific	1
transportSpecific		2
uds_address			/var/run/cmlds_server

[eth1]
delay_mechanism	P2P
</pre>