---
title: Documentation
description: "Example configuration profiles and listing of man pages for Linux PTP."
---

### Documentation

#### Example Configurations

Linux PTP provides the following example configurations (profiles) containing those attributes which differ from the [default configuration](/documentation/default). Refer to [ptp4l(8)](https://github.com/richardcochran/linuxptp/blob/45f23047d22a3ab9ca0786a34d604405780cbb89/ptp4l.8) for the definition and possible values for each configuration option.

* [E2E-TC.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/E2E-TC.cfg): End to End Transparent Clock.
* [G.8265.1.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/G.8265.1.cfg): Telecom G.8265.1 for frequency synchronization.
* [G.8275.1.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/G.8275.1.cfg): Telecom G.8275.1 for phase/time synchronization with full timing support from the network.
* [G.8275.2.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/G.8275.2.cfg): Telecom G.8275.2 for time/phase synchronization with partial timing support from the network.
* [P2P-TC.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/P2P-TC.cfg): Peer to Peer Transparent Clock.
* [UNICAST-MASTER.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/UNICAST-MASTER.cfg)
* [UNICAST-SLAVE.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/UNICAST-SLAVE.cfg)
* [automotive-master.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/automotive-master.cfg): Automotive profile for master.
* [automotive-slave.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/automotive-slave.cfg): Automotive profile for client.
* [gPTP.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/gPTP.cfg): 802.1AS for timing and synchronization of time-sensitive applications in bridged Local Area Networks". 
* [snmpd.conf](https://github.com/richardcochran/linuxptp/blob/master/configs/snmpd.conf)
* [ts2phc-TC.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/ts2phc-TC.cfg): This example shows ts2phc keeping a group of three Intel i210 cards
synchronized to each other in order to form a Transparent Clock.
* [ts2phc-generic.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/ts2phc-generic.cfg): This example uses a PPS signal from a GPS receiver as an input to the SDP0 pin of an Intel i210 card.

* * *

#### Man Pages

* [hwstamp_ctl(8)](/documentation/hwstamp_ctl/): set time stamping policy at the driver level
* [nsm(8)](/documentation/nsm/): NetSync Monitor client
* [phc2sys(8)](/documentation/phc2sys/): synchronize two or more clocks
* [phc_ctl(8)](https://github.com/richardcochran/linuxptp/blob/master/phc_ctl.8): directly control PHC device clock
* [pmc(8)](https://github.com/richardcochran/linuxptp/blob/master/pmc.8): PTP management client
* [ptp4l(8)](https://github.com/richardcochran/linuxptp/blob/master/ptp4l.8): PTP Boundary/Ordinary/Transparent Clock
* [timemaster(8)](https://github.com/richardcochran/linuxptp/blob/master/timemaster.8): run NTP with PTP as reference clocks
* [ts2phc(8)](https://github.com/richardcochran/linuxptp/blob/master/ts2phc.8): Synchronizes one or more PTP Hardware Clocks using external time stamps.