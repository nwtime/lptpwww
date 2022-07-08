---
title: Unicast Slave
description: "Example Linux PTP configuration for a unicast slave."
date: 2018-04-30
---

### Unicast Slave

Unicast Slave example configuration with contrived master tables.

> This example will not work out of the box! 

Refer to the [Unicast Discovery Options section of ptp4l(8)](/documentation/ptp4l/#unicast-discovery-options) for descriptions of the listed options.

<pre>
[global]
#
# Request service for sixty seconds.
#
unicast_req_duration	60

#
# This table has four possible UDPv4 master clocks.
#
[unicast_master_table]
table_id		1
logQueryInterval	2
UDPv4			192.168.1.11
UDPv4			192.168.2.22
UDPv4			192.168.3.33

#
# This table has just one Layer-2 master clock.
#
[unicast_master_table]
table_id		2
logQueryInterval	2
L2			00:11:22:33:44:55

#
# This table would be for use with the P2P delay mechanism.
#
[unicast_master_table]
table_id		3
logQueryInterval	2
peer_address		192.168.4.44
UDPv4			192.168.4.44

#
# eth0 uses the master table with ID 1 over UDPv4.
#
[eth0]
unicast_master_table	1

#
# eth1 uses the master table with ID 2 over Layer-2.
#
[eth1]
network_transport	L2
unicast_master_table	2
</pre>