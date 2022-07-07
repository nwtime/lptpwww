---
title: Documentation
description: "Example configuration profiles and listing of man pages for Linux PTP."
---

### Documentation

#### Example Configurations

Linux PTP provides the following example configurations (profiles) containing those attributes which differ from the [default configuration](/documentation/default). Refer to [ptp4l(8)](/documentation/ptp4l/) for the definition and possible values for each configuration option.

> **NOTE:** The settings in the [default.cfg](/documentation/configs/default-cfg/): are assumed unless overridden by a custom configuration.

##### Telecom

* [G.8265.1.cfg](/documentation/configs/g-8265-1/): Telecom G.8265.1 for frequency synchronization.
* [G.8275.1.cfg](/documentation/configs/g-8275-1/): Telecom G.8275.1 for phase/time synchronization with full timing support from the network.
* [G.8275.2.cfg](/documentation/configs/g-8275-2/): Telecom G.8275.2 for time/phase synchronization with partial timing support from the network.

##### Automotive

* [automotive-master.cfg](/documentation/configs/automotive-master/): Automotive profile for master.
* [automotive-slave.cfg](/documentation/configs/automotive-slave/): Automotive profile for client.

##### Transparent Clock

Transparent Clock is used by bridges or routers to assist clocks in measuring and adjusting for packet delay. The transparent clock computes the variable delay as the PTP packets pass through the switch or the router.

* [E2E-TC.cfg](/documentation/configs/e2e-tc/): End to End Transparent Clock.

* [P2P-TC.cfg](/documentation/configs/p2p-tc/): Peer to Peer Transparent Clock.

* [ts2phc-TC.cfg](/documentation/configs/ts2phc-tc/): This example shows ts2phc keeping a group of three Intel i210 cards
synchronized to each other in order to form a Transparent Clock.
* [ts2phc-generic.cfg](/documentation/configs/ts2phc-generic/): This example uses a PPS signal from a GPS receiver as an input to the SDP0 pin of an Intel i210 card.

##### Miscellaneous

* [UNICAST-MASTER.cfg](/documentation/configs/unicast-master/)
* [UNICAST-SLAVE.cfg](/documentation/configs/unicast-slave/)

* [gPTP.cfg](/documentation/configs/gptp/): 802.1AS for timing and synchronization of time-sensitive applications in bridged Local Area Networks. 
* [snmpd.conf](/documentation/configs/snmpd-conf)


* * *

#### Man Pages

* [hwstamp_ctl(8)](/documentation/hwstamp_ctl/): set time stamping policy at the driver level
* [nsm(8)](/documentation/nsm/): NetSync Monitor client
* [phc2sys(8)](/documentation/phc2sys/): synchronize two or more clocks
* [phc_ctl(8)](/documentation/phc_ctl/): directly control PHC device clock
* [pmc(8)](/documentation/pmc/): PTP management client
* [ptp4l(8)](/documentation/ptp4l/): PTP Boundary/Ordinary/Transparent Clock
* [timemaster(8)](/documentation/timemaster/): run NTP with PTP as reference clocks
* [ts2phc(8)](/documentation/ts2phc/): Synchronizes one or more PTP Hardware Clocks using external time stamps