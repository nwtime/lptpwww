---
title: "Features"
description: "Listing of features supported by Linux PTP software."
---

### Features

  - Supports hardware and software time stamping via the Linux
    `SO_TIMESTAMPING` socket option.

  - Supports the Linux PTP Hardware Clock (PHC) subsystem by using the
    `clock_gettime` family of calls, including the `clock_adjtimex` system
    call.

  - Implements Boundary Clock (BC), Ordinary Clock (OC) and
    Transparent Clock (TC).

  - Transport over UDP/IPv4, UDP/IPv6, and raw Ethernet (Layer 2).

  - Supports [IEEE 802.1AS-2011](https://standards.ieee.org/ieee/802.1AS/3956/) in the role of end station.

  - Modular design allowing painless addition of new transports and
    clock servos.

  - Implements unicast operation.

  - Supports a number of profiles, including:

    - The automotive profile.

    - The default 1588 profile.

    - The enterprise profile.

    - The telecom profiles [G.8265.1](/documentation/configs/g-8265-1/), [G.8275.1](/documentation/configs/g-8275-1/), and [G.8275.2](/documentation/configs/g-8275-2/).

  - Supports the [NetSync Monitor protocol](https://www.meinbergglobal.com/english/news/optimize-network-synchronization-with-the-netsync-monitor.htm).

  - Implements Peer to Peer one-step time stamping.

  - Supports bonded, IPoIB, and vlan interfaces.

&nbsp; 

  