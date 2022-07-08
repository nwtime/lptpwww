---
title: Configs
description: "Example configuration profiles for Linux PTP."
---

#### Example Configurations

Linux PTP provides the following example configurations (profiles) containing those attributes which differ from the [default configuration](/documentation/default/). Refer to [ptp4l(8)](/documentation/ptp4l/) for the definition and possible values for each configuration option.

> **NOTE:** The settings in the [default.cfg](/documentation/configs/default-cfg/): are assumed unless overridden by a custom configuration.

##### Telecom

* [G.8265.1/](/documentation/configs/g-8265-1/): Telecom G.8265.1 for frequency synchronization.
* [G.8275.1/](/documentation/configs/g-8275-1/): Telecom G.8275.1 for phase/time synchronization with full timing support from the network.
* [G.8275.2/](/documentation/configs/g-8275-2/): Telecom G.8275.2 for time/phase synchronization with partial timing support from the network.

##### Automotive

* [automotive-master/](/documentation/configs/automotive-master/): Automotive profile for master.
* [automotive-slave/](/documentation/configs/automotive-slave/): Automotive profile for client.

##### Transparent Clock

Transparent Clock is used by bridges or routers to assist clocks in measuring and adjusting for packet delay. The transparent clock computes the variable delay as the PTP packets pass through the switch or the router.

* [E2E-TC/](/documentation/configs/e2e-tc/): End to End Transparent Clock.

* [P2P-TC/](/documentation/configs/p2p-tc/): Peer to Peer Transparent Clock.

* [ts2phc-TC/](/documentation/configs/ts2phc-tc/): This example shows ts2phc keeping a group of three Intel i210 cards
synchronized to each other in order to form a Transparent Clock.
* [ts2phc-generic/](/documentation/configs/ts2phc-generic/): This example uses a PPS signal from a GPS receiver as an input to the SDP0 pin of an Intel i210 card.

##### Miscellaneous

* [UNICAST-MASTER/](/documentation/configs/unicast-master/)
* [UNICAST-SLAVE/](/documentation/configs/unicast-slave/)

* [gPTP/](/documentation/configs/gptp/): 802.1AS for timing and synchronization of time-sensitive applications in bridged Local Area Networks. 
* [snmpd.conf](/documentation/configs/snmpd-conf/)
