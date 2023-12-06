---
title: "ptp4l(8): PTP Boundary/Ordinary/Transparent Clock"
description: "Man page for ptp4l, an implementation of the Precision Time Protocol (PTP) according to IEEE standard 1588 for Linux."
date: 2023-05-10
---

### ptp4l(8): PTP Boundary/Ordinary/Transparent Clock

#### SYNOPSIS

<code>ptp4l [ -AEP246HSLmqsv ] [ -f _config_ ] [ -p _phc-device_ ] [ -l _print-level_ ] [ -i _interface_ ] [ _long-options_ ] ...</code>

#### DESCRIPTION

`ptp4l` is an implementation of the Precision Time Protocol (PTP) according to IEEE standard 1588 for Linux. It implements Boundary Clock (BC), Ordinary Clock (OC), and Transparent Clock (TC).

#### OPTIONS

<code>**-A**</code>

: Select the delay mechanism automatically. Start with E2E and switch to P2P when a peer delay request is received.

<code>**-E**</code>

: Select the delay request-response (E2E) mechanism. This is the default mechanism. All clocks on single PTP communication path must use the same mechanism. A warning will be printed when a peer delay request is received on port using the E2E mechanism.

<code>**-P**</code>

: Select the peer delay (P2P) mechanism. A warning will be printed when a delay request is received on port using the P2P mechanism.

<code>**-2**</code>

: Select the IEEE 802.3 network transport.

<code>**-4**</code>

: Select the UDP IPv4 network transport. This is the default transport.

<code>**-6**</code>

: Select the UDP IPv6 network transport.

<code>**-H**</code>

: Select the hardware time stamping. All ports specified by the `-i` option and in the configuration file must be attached to the same PTP hardware clock (PHC). This is the default time stamping.

<code>**-S**</code>

: Select the software time stamping.

<code>**-L**</code>

: Select the legacy hardware time stamping.

<code>**-f _config_**</code>

: Read configuration from the specified file. No configuration file is read by default.

<code>**-i _interface_**</code>

: Specify a PTP port, it may be used multiple times. At least one port must be specified by this option or in the configuration file.

<code>**-p _phc-device_**</code>

: **(This option is deprecated.)** Before Linux kernel v3.5 there was no way to discover the PHC device associated with a network interface.  This option specifies the PHC device (e.g. `/dev/ptp0`) to be used when running on legacy kernels.

<code>**-s**</code>

Enable the `clientOnly` mode.

<code>**-l _print-level_**</code>

: Set the maximum `syslog` level of messages which should be printed or sent to the system logger. The default is 6 (`LOG_INFO`).

<code>**-m**</code>

: Print messages to the standard output.

<code>**-q**</code>

: Don't send messages to the system logger.

<code>**-v**</code>

: Prints the software version and exits.

<code>**-h**</code>

: Display a help message.

#### LONG OPTIONS

Each and every configuration file option (see below) may also appear as a "long" style command line argument.  For example, the `clientOnly` option may be set using either of these two forms.

`--clientOnly 1`

`--clientOnly=1`

Option values given on the command line override values in the global section of the configuration file.

#### CONFIGURATION FILE

The configuration file is divided into sections. Each section starts with a line containing its name enclosed in brackets and it follows with settings. Each setting is placed on a separate line, it contains the name of the option and the value separated by whitespace characters. Empty lines and lines starting with `#` are ignored.

There are three different section types.

1. The global section (indicated as `[global]`) sets the program options, clock options and default port options. Other sections are port specific sections and they override the default port options.

2. Port sections give the name of the configured port (e.g. `[eth0]`). Ports specified in the configuration file don't need to be specified by the `-i` option. An empty port section can be used to replace the command line option.

3. Tables for configuring unicast discovery begin with `%[unicast_master_table]`. See [UNICAST DISCOVERY OPTIONS](#unicast-discovery-options), below.

#### PORT OPTIONS

<code>**announceReceiptTimeout**</code>

: The number of missed Announce messages before the last Announce messages expires. The default is 3.

<code>**boundary_clock_jbod**</code>

: When running as a boundary clock (that is, when more than one network interface is configured), `ptp4l` performs a sanity check to make sure that all of the ports share the same hardware clock device. This option allows `ptp4l` to work as a boundary clock using "just a bunch of devices" that are not synchronized to each other. For this mode, the collection of clocks must be synchronized by an external program, for example [phc2sys(8)](/documentation/phc2sys/) in `automatic` mode. The default is 0 (disabled).

<code>**delayAsymmetry**</code>

: The time difference in nanoseconds of the transmit and receive paths. This value should be positive when the server-to-client propagation time is longer and negative when the client-to-server time is longer. The default is 0 nanoseconds.

<code>**delay_filter**</code>

: Select the algorithm used to filter the measured delay and peer delay. Possible values are `moving_average` and `moving_median`. The default is `moving_median`.

<code>**delay_filter_length**</code>

: The length of the delay filter in samples. The default is 10.

<code>**delay_mechanism**</code>

: Select the delay mechanism. Possible values are `E2E`, `P2P`, `NONE`, and `Auto`. The default is `E2E`.

<code>**delay_response_timeout**</code>

: The number of delay response messages that may go missing before triggering a synchronization fault. Setting this option to zero will disable the delay response timeout. The default is 0 or disabled.

<code>**egressLatency**</code>

: Specifies the difference in nanoseconds between the actual transmission time at the reference plane and the reported transmit time stamp. This value will be added to egress time stamps obtained from the hardware. The default is 0.

<code>**fault_badpeernet_interval**</code>

: The time in seconds between the detection of a peer network misconfiguration and the fault being reset. The port is disabled for the duration of the interval. The value is in seconds and the special key word `ASAP` will let the fault be reset immediately. The default is 16 seconds.

<code>**fault_reset_interval**</code>

: The time in seconds between the detection of a port's fault and the fault being reset. This value is expressed as a power of two. Setting this value to `-128` or to the special key word `ASAP` will let the fault be reset immediately. The default is 4 (16 seconds).

<code>**follow_up_info**</code>

: Include the 802.1AS data in the `Follow_Up` messages if enabled. The default is 0 (disabled).

<code>**G.8275.portDS.localPriority**</code>

: The Telecom Profiles (ITU-T [G.8275.1](/documentation/configs/g-8275-1/) and [G.8275.2](/documentation/configs/g-8275-2/)) specify an alternate Best Master Clock Algorithm (BMCA) with a unique data set comparison algorithm.  The value of this option is associated with Announce messages arriving on a particular port and is used as a tie breaker whenever `clockClass`, `clockAccuracy`, `offsetScaledLogVariance`, and `priority2` are equal. This option is only used when `dataset_comparison` is set to `G.8275.x`. The default value is 128.

> **Warning:** the BMCA is guaranteed to produce a spanning tree (that is, a timing network without loops) only when using the default values of `G.8275.defaultDS.localPriority` and `G.8275.portDS.localPriority`. Careful network engineering is needed when using non-default values.

<code>**hybrid_e2e**</code>

: Enables the `hybrid` delay mechanism from the draft Enterprise Profile. When enabled, ports in the client state send their delay request messages to the unicast address taken from the server's announce message. Ports in the server state will reply to unicast delay requests using unicast delay responses. This option has no effect if the delay_mechanism is set to `P2P`. The default is 0 (disabled).

<code>**ignore_transport_specific**</code>

: By default, incoming messages are dropped if their `transportSpecific` field does not match the configured value.  However, many transports specified in the 1588 standard mandate ignoring this field. Moreover, some equipment is known to set the reserved bits. Configuring this option as 1 causes this field to be ignored completely on receive.  The default is 0.

<code>**ingressLatency**</code>

: Specifies the difference in nanoseconds between the reported receive time stamp and the actual reception time at reference plane. This value will be subtracted from ingress time stamps obtained from the hardware. The default is 0.

<code>**inhibit_delay_req**</code>

: Don't send any delay requests. This will need the `asCapable` config option to be set to `true`. This is useful when running as a designated server who does not need to calculate offset from client. The default is 0 (disabled).

<code>**inhibit_multicast_service**</code>

: Some unicast mode profiles insist that no multicast message are ever transmitted.  Setting this option inhibits multicast transmission. The default is 0 (mutlicast enabled).

<code>**logAnnounceInterval**</code>

: The mean time interval between Announce messages. A shorter interval makes `ptp4l` react faster to the changes in the client/server hierarchy. The interval should be the same in the whole domain. It's specified as a power of two in seconds. The default is 1 (2 seconds).

<code>**logMinDelayReqInterval**</code>

: The minimum permitted mean time interval between `Delay_Req` messages. A shorter interval makes `ptp4l` react faster to the changes in the path delay. It's specified as a power of two in seconds. The default is 0 (1 second).

<code>**logMinPdelayReqInterval**</code>

: The minimum permitted mean time interval between `Pdelay_Req` messages. It's specified as a power of two in seconds. The default is 0 (1 second).

<code>**logSyncInterval**</code>

: The mean time interval between Sync messages. A shorter interval may improve accuracy of the local clock. It's specified as a power of two in seconds. The default is 0 (1 second).

<code>**masterOnly**</code>

: This option is deprecated and will be removed in a future release. Use `serverOnly` instead.

<code>**min_neighbor_prop_delay**</code>

: Lower limit for peer delay in nanoseconds. If the estimated peer delay is smaller than this value the port is marked as not 802.1AS capable.

<code>**neighborPropDelayThresh**</code>

: Upper limit for peer delay in nanoseconds. If the estimated peer delay is greater than this value the port is marked as not 802.1AS capable.

<code>**network_transport**</code>

Select the network transport. Possible values are `UDPv4`, `UDPv6`, and `L2`. The default is `UDPv4`.

<code>**net_sync_monitor**</code>

: Enables the NetSync Monitor (NSM) protocol. The NSM protocol allows a station to measure how well another node is synchronized. The monitor sends a unicast delay request to the node, which replies unconditionally with unicast delay response, sync, and follow up messages. If the monitor is synchronized to the GM, it can use the time stamps in the message to estimate the node's offset.  This option requires that the `hybrid_e2e` option be enabled as well. The default is 0 (disabled).

<code>**operLogPdelayReqInterval**</code>

: The Pdelay Request messages interval to be used once the clock enters the `SERVO_LOCKED_STABLE` state.  If the `msg_interval_request` option is set, then the local client port will adopt this rate when the local clock enters the "locked stable" state.  This option is specified as a power of two in seconds, and the default value is 0 (1 second).

<code>**operLogSyncInterval**</code>

: The Sync message interval to be requested once the clock enters the `SERVO_LOCKED_STABLE` state.  If the `msg_interval_request` option is set, then the local client port will request the remote server to switch to the given message rate via a signaling message containing a Message interval request TLV.  This option is specified as a power of two in seconds, and default value is 0 (1 second).

<code>**path_trace_enabled**</code>

: Enable the mechanism used to trace the route of the Announce messages. The default is 0 (disabled).

<code>**phc_index**</code>

: Specifies the index of the PHC to be used for synchronization with hardware timestamping. This option is useful with virtual clocks running on top of a free-running physical clock (created by writing to `/sys/class/ptp/ptp*/n_vclocks`). The default is -1, which means the index will be set to the PHC associated with the interface, or the device specified by the `-p` option.

<code>**power_profile.2011.grandmasterTimeInaccuracy**</code>

: Specifies the time inaccuracy of the GM in nanoseconds.  Relevant only when power_profile.version is 2011.  This value may be changed
dynamically using the `POWER_PROFILE_SETTINGS_NP` management message. The default is -1 meaning unknown inaccuracy.

<code>**power_profile.2011.networkTimeInaccuracy**</code>

: Specifies the time inaccuracy of the network in nanoseconds.  Relevant only when `power_profile.version` is 2011.  This value may be changed
dynamically using the `POWER_PROFILE_SETTINGS_NP` management message. The default is -1 meaning unknown inaccuracy.

<code>**power_profile.2017.totalTimeInaccuracy**</code>

: Specifies the sum of the GM, network, and local node inaccuracies in nanoseconds.  Relevant only when `power_profile.version` is 2017.  This
value may be changed dynamically using the `POWER_PROFILE_SETTINGS_NP` management message.  The default is -1 meaning unknown inaccuracy.

<code>**power_profile.grandmasterID**</code>

: Specifies an optional, non-zero identification code for the GM.  Note that the code is an arbitrary, power profile specific integer, not
necessarily related to the clockIdentity in any way.  This value may be changed dynamically using the `POWER_PROFILE_SETTINGS_NP` management
message.  The default is 0 meaning unused.

<code>**power_profile.version**</code>

: Specifies the power profile version to be used.  Valid values are `none`, `2011`, or `2017`. This value may be changed dynamically using the
`POWER_PROFILE_SETTINGS_NP` management message. The default is `none`.

<code>**ptp_dst_mac**</code>

: The MAC address to which PTP messages should be sent. Relevant only with `L2` transport. The default is `01:1B:19:00:00:00`.

<code>**p2p_dst_mac**</code>

: The MAC address to which peer delay messages should be sent. Relevant only with `L2` transport. The default is `01:80:C2:00:00:0E`.

<code>**serverOnly**</code>

: Setting this option to `1` prevents the port from entering the client state. In addition, the local clock will ignore Announce messages received on this port. This option's intended use is to support the Telecom Profiles according to ITU-T G.8265.1, G.8275.1, and G.8275.2. The default value is `0` or false.

<code>**syncReceiptTimeout**</code>

: The number of sync/follow up messages that may go missing before triggering a Best Master Clock election. This option is used for running in `gPTP` mode according to the 802.1AS-2011 standard. Setting this option to zero will disable the sync message timeout. The default is 0 or disabled.

<code>**transportSpecific**</code>

: The transport specific field. Must be in the range 0 to 255. The default is 0.

<code>**tsproc_mode**</code>

: Select the time stamp processing mode used to calculate offset and delay. Possible values are `filter`, `raw`, `filter_weight`, `raw_weight`. Raw modes perform well when the rate of sync messages (`logSyncInterval`) is similar to the rate of delay messages (`logMinDelayReqInterval` or `logMinPdelayReqInterval`). Weighting is useful with larger network jitters (e.g. software time stamping). The default is `filter`.

<code>**udp_ttl**</code>

: Specifies the Time to live (TTL) value for IPv4 multicast messages and the hop limit for IPv6 multicast messages. This option is only relevant with the IPv4 and IPv6 UDP transports. The default is 1 to restrict the messages sent by `ptp4l` to the same subnet.

<code>**unicast_listen**</code>

: When enabled, this option allows the port to grant unicast message contracts.  Incoming requests will be granted, limited only by the amount of memory available. The default is 0 (disabled).

<code>**unicast_master_table**</code>

: When set to a positive integer, this option specifies the table id to be used for unicast discovery.  Each table lives in its own section and has a unique, positive numerical ID.  Entries in the table are a pair of transport type and protocol address. The default is 0 (unicast discovery disabled).

<code>**unicast_req_duration**</code>

: The service time in seconds to be requested during unicast discovery. Note that the remote node is free to grant a different duration. The default is 3600 seconds or one hour.

#### PROGRAM AND CLOCK OPTIONS

<code>**asCapable**</code>

: If set to `true`, all the checks which can unset the `asCapable` variable (as described in Section 10.2.4.1 of 802.1AS) are skipped. If set to `auto`, `asCapable` is initialized to `false` and will be set to `true` after the relevant checks have passed. The default value is `auto`.

<code>**assume_two_step**</code>

: Treat one-step responses as two-step if enabled. It is used to work around buggy 802.1AS switches. The default is 0 (disabled).

<code>**BMCA**</code>

: This option enables use of static roles for server and client devices instead of running the best master clock algorithm (BMCA) described in the 1588 profile. This can be used to speed up the start time for servers and clients when you know the roles of the devices in advance.  When set to `noop`, the traditional BMCA algorithm used by 1588 is skipped. `masterOnly` and `clientOnly` will be used to determine the server or client role for the device. In a bridge, `clientOnly` (which is a global option) can be set to make all ports assume the client role. `masterOnly` (which is a per-port config option) can then be used to set individual ports to take on the server role. The default value is `ptp` which runs the BMCA related state machines.

<code>**check_fup_sync**</code>

: Because of packet reordering that can occur in the network, in the hardware, or in the networking stack, a follow up message can appear to arrive in the application before the matching sync message. As this is a normal occurrence, and the sequenceID message field ensures proper matching, the `ptp4l` program accepts out of order packets. This option adds an additional check using the software time stamps from the networking stack to verify that the sync message did arrive first. This option is only useful if you do not trust the sequence IDs generated by the server. The default is 0 (disabled).

<code>**clientOnly**</code>

: The local clock is a client-only clock if enabled. The default is 0 (disabled).

<code>**clockAccuracy**</code>

: The `clockAccuracy` attribute of the local clock. It is used in the PTP server selection algorithm. The default is `0xFE`.

<code>**clockClass**</code>

: The `clockClass` attribute of the local clock. It denotes the traceability of the time distributed by the grandmaster clock. The default is 248.

<code>**clock_class_threshold**</code>

: The maximum clock class value from master, acceptible to sub-ordinate clock beyond which it moves out of lock state. The default value is 248.

<code>**clockIdentity**</code>

: The `clockIdentity` attribute of the local clock. The clockIdentity is an 8-octet array and should in this configuration be written in textual form, see default. It should be unique since it is used to identify the specific clock. If default is used or if not set at all, the `clockIdentity` will be automtically generated. The default is `000000.0000.000000`.

<code>**clock_servo**</code>

: The servo which is used to synchronize the local clock. Valid values are `pi` for a PI controller, `linreg` for an adaptive controller using linear regression, `ntpshm` and `refclock_sock` for the NTP SHM and chrony SOCK reference clocks respectively to allow another process to synchronize the local clock, and `nullf` for a servo that always dials frequency offset zero (for use in SyncE nodes). The default is `pi`.

<code>**clock_type**</code>

: Specifies the kind of PTP clock.  Valid values are `OC` for ordinary clock, `BC` for boundary clock, `P2P_TC` for peer to peer transparent clock, and `E2E_TC` for end to end transparent clock.  A multi-port ordinary clock will automatically be configured as a boundary clock. The default is `OC`.

<code>**dataset_comparison**</code>

: Specifies the method to be used when comparing data sets during the Best Master Clock Algorithm.  The possible values are `ieee1588` and `G.8275.x`.  The default is `ieee1588`.

<code>**domainNumber**</code>

: The domain attribute of the local clock. The default is 0.

<code>**dscp_event**</code>

: Defines the Differentiated Services Codepoint (DSCP) to be used for PTP event messages. Must be a value between 0 and 63. There are several media streaming standards out there that require specific values for this option. For example 46 (EF PHB) in AES67 or 48 (CS6 PHB) in RAVENNA. The default is 0.

<code>**dscp_general**</code>

: Defines the Differentiated Services Codepoint (DSCP) to be used for PTP general messages. Must be a value between 0 and 63. There are several media streaming standards out there that recommend specific values for this option. For example 34 (AF41 PHB) in AES67 or 46 (EF PHB) in RAVENNA. The default is 0.

<code>**first_step_threshold**</code>

: The maximum offset the servo will correct by changing the clock frequency (phase when using `nullf` servo) instead of stepping the clock. This is only applied on the first update. It's specified in seconds. When set to 0.0, the servo won't step the clock on start. The default is 0.00002 (20 microseconds). This option used to be called `pi_f_offset_const`.

<code>**free_running**</code>

: Don't adjust the local clock if enabled. The default is 0 (disabled).

<code>**freq_est_interval**</code>

: The time interval over which is estimated the ratio of the local and peer clock frequencies. It is specified as a power of two in seconds. The default is 1 (2 seconds).

<code>**G.8275.defaultDS.localPriority**</code>

: The Telecom Profiles (ITU-T [G.8275.1](/documentation/configs/g-8275-1/) and [G.8275.2](/documentation/configs/g-8275-2/)) specify an alternate Best Master Clock Algorithm (BMCA) with a unique data set comparison algorithm.  The value of this option is associated with the local clock and is used as a tie breaker whenever `clockClass`, `clockAccuracy`, `offsetScaledLogVariance`, and `priority2` are equal. This option is only used when `dataset_comparison` is set to `G.8275.x`. The default value is 128.

> **Warning:** the BMCA is guaranteed to produce a spanning tree (that is, a timing network without loops) only when using the default values of `G.8275.defaultDS.localPriority` and `G.8275.portDS.localPriority`. Careful network engineering is needed when using non-default values.

<code>**gmCapable**</code>

: If this option is enabled, then the local clock is able to become grand master. This is only for use with 802.1AS clocks and has no effect on 1588 clocks. The default is 1 (enabled).

<code>**hwts_filter**</code>

: Select the hardware time stamp filter setting mode. Possible values are `normal`, `check`, and `full`. Normal mode sets the filters as needed. Check mode only checks but does not set. Full mode sets the receive filter to mark all packets with hardware time stamp, so all applications can get them. The default is `normal`.

<code>**ignore_source_id**</code>

: This will disable source port identity checking for Sync and Follow_Up messages. This is useful when the announce messages are disabled in the server and the client does not have any way to know the server's identity. The default is 0 (disabled).

<code>**inhibit_announce**</code>

: This will disable the timer for announce messages (i.e. `FD_MANNO_TIMER`) and also the announce message timeout timer (i.e. `FD_ANNOUNCE_TIMER`). This is used by the Automotive profile as part of switching over to a static BMCA.  If this option is enabled, `ignore_source_id` has to be enabled in the client because it has no way to identify the server in the Sync and Follow_Up messages. The default is 0 (disabled).

<code>**initial_delay**</code>

: The initial path delay of the clock in nanoseconds used for synchronization of the clock before the delay is measured using the `E2E` or `P2P` delay mechanism. If set to 0, the clock will not be updated until the delay is measured. The default is 0.

<code>**interface_rate_tlv**</code>

: When the client and server are operating are operating at different interface rate, delay asymmetry caused due to different interface rate needs to be compensated. The server sends its interface rate using interface rate TLV as per G.8275.2 Annex D. The default is 0 (does not support interface rate tlv).

<code>**kernel_leap**</code>

: When a leap second is announced, let the kernel apply it by stepping the clock instead of correcting the one-second offset with servo, which would correct the one-second offset slowly by changing the clock frequency (unless the `step_threshold` option is set to correct such offset by stepping). Relevant only with software time stamping. The default is 1 (enabled).

<code>**logging_level**</code>

: The maximum logging level of messages which should be printed. The default is 6 (`LOG_INFO`).

<code>**manufacturerIdentity**</code>

: The manufacturer id which should be an OUI owned by the manufacturer. The default is `00:00:00`.

<code>**max_frequency**</code>

: The maximum allowed frequency adjustment of the clock in parts per billion (ppb). This is an additional limit to the maximum allowed by the hardware. When set to 0, the hardware limit will be used. The default is 900000000 (90%). This option used to be called `pi_max_frequency`.

<code>**maxStepsRemoved**</code>

: When using this option, if the value of `stepsRemoved` of an Announce message is greater than or equal to the value of `maxStepsRemoved`, the Announce message is not considered in the operation of the BMCA. The default value is 255.

<code>**message_tag**</code>

: The tag which is added to all messages printed to the standard output or system log. If the tag contains the string `"{level}"`, it will be replaced with the log level of the message as a number. The default is an empty string (which cannot be set in the configuration file as the option requires an argument).

<code>**msg_interval_request**</code>

: This option, when set, will trigger an adjustment to the Sync and peer delay request message intervals when the clock servo transitions into the `SERVO_LOCKED_STABLE` state.  The Sync interval will be adjusted via the signaling mechanism while the `pdelay` request interval is simply adjusted locally.  The values to use for the new Sync and peer delay request intervals are specified by the `operLogSyncInterval` and `operLogPdelayReqInterval` options, respectively.
The default value of `msg_interval_request` is 0 (disabled).

<code>**ntpshm_segment**</code>

: The number of the SHM segment used by `ntpshm` servo. The default is 0.

<code>**offsetScaledLogVariance**</code>

: The `offsetScaledLogVariance` attribute of the local clock. It characterizes the stability of the clock. The default is `0xFFFF`.

<code>**pi_integral_const**</code>

: The integral constant of the PI controller. When set to 0.0, the integral constant will be set by the following formula from the current sync interval. The default is 0.0.

&emsp;&emsp; `ki = min(ki_scale * sync^ki_exponent, ki_norm_max / sync)`

<code>**pi_integral_exponent**</code>

: The `ki_exponent` constant in the formula used to set the integral constant of the PI controller from the sync interval. The default is 0.4.

<code>**pi_integral_norm_max**</code>

: The `ki_norm_max` constant in the formula used to set the integral constant of the PI controller from the sync interval. The default is 0.3.

<code>**pi_integral_scale**</code>

: The `ki_scale` constant in the formula used to set the integral constant of the PI controller from the sync interval. When set to 0.0, the value will be selected from 0.3 and 0.001 for the hardware and software time stamping respectively. The default is 0.0.

<code>**pi_proportional_const**</code>

: The proportional constant of the PI controller. When set to 0.0, the proportional constant will be set by the following formula from the current sync interval. The default is 0.0.

&emsp;&emsp; `kp = min(kp_scale * sync^kp_exponent, kp_norm_max / sync)`

<code>**pi_proportional_exponent**</code>

: The `kp_exponent` constant in the formula used to set the proportional constant of the PI controller from the sync interval. The default is -0.3.

<code>**pi_proportional_norm_max**</code>

: The `kp_norm_max` constant in the formula used to set the proportional constant of the PI controller from the sync interval. The default is 0.7

<code>**pi_proportional_scale**</code>

: The `kp_scale` constant in the formula used to set the proportional constant of the PI controller from the sync interval. When set to 0.0, the value will be selected from 0.7 and 0.1 for the hardware and software time stamping respectively. The default is 0.0.

<code>**priority1**</code>

: The `priority1` attribute of the local clock. It is used in the PTP server selection algorithm, lower values take precedence. Must be in the range 0 to 255. The default is 128.

<code>**priority2**</code>

: The `priority2` attribute of the local clock. It is used in the PTP server selection algorithm, lower values take precedence. Must be in the range 0 to 255. The default is 128.

<code>**productDescription**</code>

: The product description string. Allowed values must be of the form `manufacturerName;modelNumber;instanceIdentifier` and contain at most 64 utf8 symbols. The default is `;;`.

<code>**ptp_minor_version**</code>

: This option sets the `minorVersionPTP` in the common PTP message header. The default is `1`.

<code>**refclock_sock_address**</code>

: The address of the UNIX domain socket to be used by the `refclock_sock` servo. The default is `/var/run/refclock.ptp.sock`.

<code>**revisionData**</code>

: The revision description string which contains the revisions for node hardware (HW), firmware (FW), and software (SW). Allowed values are of the form `HW;FW;SW` and contain at most 32 utf8 symbols. The default is `;;`.

<code>**sanity_freq_limit**</code>

: The maximum allowed frequency offset between uncorrected clock and the system monotonic clock in parts per billion (ppb). This is used as a sanity check of the synchronized clock. When a larger offset is measured, a warning message will be printed and the servo will be reset. If the frequency correction set by `ptp4l` changes unexpectedly between updates of the clock (e.g. due to another process trying to control the clock), a warning message will be printed. When set to 0, the sanity check is disabled. The default is 200000000 (20%).

<code>**servo_num_offset_values**</code>

: The number of offset values considered in order to transition from the `SERVO_LOCKED` to the `SERVO_LOCKED_STABLE` state. The transition occurs once the last `servo_num_offset_values` offsets are all below the `servo_offset_threshold` value. The default value is 10.

<code>**servo_offset_threshold**</code>

: The offset threshold used in order to transition from the `SERVO_LOCKED` to the `SERVO_LOCKED_STABLE` state.  The transition occurs once the last `servo_num_offset_values` offsets are all below the threshold value. The default value of `offset_threshold` is 0 (disabled).

<code>**slave_event_monitor**</code>

: Specifies the address of a UNIX domain socket for event monitoring.  A local monitoring client bound to this address will receive `SLAVE_RX_SYNC_TIMING_DATA` and `SLAVE_DELAY_TIMING_DATA_NP` TLVs. The default is the empty string (disabled).

<code>**slaveOnly**</code>

: This option is deprecated and will be removed in a future release. Use `clientOnly` instead.

<code>**socket_priority**</code>

: Configure the `SO_PRIORITY` of sockets. This is to support cases where a user
wants to route `ptp4l` traffic using Linux `qdiscs` for the purpose of traffic
shaping. This option is only available with the IEEE 802.3 transport (the `-2` option) and is silently ignored when using the UDP IPv4/6 network transports. Must be in the range of 0 to 15, inclusive. The default is 0.

<code>**step_threshold**</code>

: The maximum offset the servo will correct by changing the clock frequency (phase when using `nullf` servo) instead of stepping the clock. When set to 0.0, the servo will never step the clock except on start. It's specified in seconds. The default is 0.0. This option used to be called `pi_offset_const`.

<code>**step_window**</code>

: When set, indicates the number of Sync events after a clock step that the clock will not do any frequency or step adjustments. This is used in situations where clock stepping is unable to happen instantaneously so there is a lag before the timestamps can settle properly to reflect the clock step. The default is 0 (disabled).

<code>**summary_interval**</code>

: The time interval in which are printed summary statistics of the clock. It is specified as a power of two in seconds. The statistics include offset root mean square (RMS), maximum absolute offset, frequency offset mean and standard deviation, and path delay mean and standard deviation. The units are nanoseconds and parts per billion (ppb). If there is only one clock update in the interval, the sample will be printed instead of the statistics. The messages are printed at the `LOG_INFO` level. The default is 0 (1 second).

<code>**tc_spanning_tree**</code>

: When running as a Transparent Clock, increment the `stepsRemoved` field of Announce messages that pass through the switch.  Enabling this option ensures that PTP message loops never form, provided the switches all implement this option together with the BMCA.

<code>**timeSource**</code>

: The time source is a single byte code that gives an idea of the kind of local clock in use. The value is purely informational, having no effect on the outcome of the Best Master Clock algorithm, and is advertised when the clock becomes grand master.

<code>**time_stamping**</code>

: The time stamping method to be used.  The allowed values are `hardware`, `software`, `legacy`, `onestep`, and `p2p1step`. The default is `hardware`.

<code>**twoStepFlag**</code>

: Enable two-step mode for sync messages. One-step mode can be used only with hardware time stamping. The default is 1 (enabled).

<code>**tx_timestamp_timeout**</code>

: The number of milliseconds to poll waiting for the tx time stamp from the kernel when a message has recently been sent. The default is 10.

<code>**udp6_scope**</code>

: Specifies the desired scope for the IPv6 multicast messages.  This will be used as the second byte of the primary address.  This option is only relevant with IPv6 transport.  See [RFC 4291](https://www.rfc-editor.org/rfc/rfc4291.html).  The default is `0x0E` for the global scope.

<code>**uds_address**</code>

: Specifies the address of the UNIX domain socket for receiving local management messages. The default is `/var/run/ptp4l`.

<code>**uds_file_mode**</code>

: File mode of the UNIX domain socket used for receiving local management messages. The mode should be specified as an octal number, i.e. it should start with a 0 literal. The default mode is `0660`.

<code>**uds_ro_address**</code>

: Specifies the address of the second UNIX domain socket for receiving local management messages, which is restricted to `GET` actions and does not forward messages to other ports. Access to this socket can be given to untrusted applications for monitoring purposes. The default is `/var/run/ptp4lro`.

<code>**uds_ro_file_mode**</code>

: File mode of the second (read-only) UNIX domain socket used for receiving local management messages. The mode should be specified as an octal number, i.e. it should start with a 0 literal. The default mode is `0666`.

<code>**use_syslog**</code>

: Print messages to the system log if enabled. The default is 1 (enabled).

<code>**userDescription**</code>

: The user description string. Allowed values are of the form `name;location` and contain at most 128 utf8 symbols. The default is an empty string.

<code>**utc_offset**</code>

: The current offset between TAI and UTC. The default is 37.

<code>**verbose**</code>

: Print messages to the standard output if enabled. The default is 0 (disabled).

<code>**write_phase_mode**</code>

: This option enables using the "write phase" feature of a PTP Hardware Clock.  If supported by the device, this mode uses the hardware's built in phase offset control instead of frequency offset control. The default value is 0 (disabled).

#### UNICAST DISCOVERY OPTIONS

<code>**L2|UDPv4|UDPv6**</code>

: Each table entry specifies the transport type and network address of a potential remote server.  If multiple servers are specified, then unicast negotiation will be performed with each of them.

<code>**logQueryInterval**</code>

: This option configures the time to wait between unicast negotiation attempts.  It is specified as a power of two in seconds. The default is 0 (1 second).

<code>**peer_address**</code>

: This option specifies the unicast address of the peer for use with the peer to peer delay mechanism.  If specified, the port owning the table will negotiate unicast peer delay responses from the machine at the given remote address, otherwise the port will send multicast peer delay requests.

<code>**table_id**</code>

: Each table must begin with a unique, positive table ID.  The port that claims a given table does so by including the ID as the value of its `unicast_master_table` option.

#### TIME SCALE USAGE

When `ptp4l` acts as the domain server, it either uses the PTP or the UTC time scale depending on time stamping mode.  In software and legacy time stamping modes it announces Arbitrary time scale mode, which is effectively UTC here.  In hardware time stamping mode it announces use of PTP time scale.

When `ptp4l` is the domain server using hardware time stamping, it is up to `phc2sys` to maintain the correct offset between UTC and PTP times. See the 
[phc2sys(8)](/documentation/phc2sys/) manual page for more details.

#### KTHREAD PRIORITY

In case of following log,

`timed out while polling for tx timestamp increasing tx_timestamp_timeout or increasing kworker priority may correct this issue, but a driver bug likely causes it`

one possible cause is that the kworker which processes timestamps is being starved.  The system admin might try manually increasing the priority of the kworker.

Many device drivers use kworker threads created by the PTP stack. Such kworkers are named:

`ptp<decimal number of clock>`

The system admin can manually bump the priority of the kworker process using `chrt`.

Example:

`pgrep -f "ptp[0-9]+" | xargs -I {} sudo chrt -f --pid 75 {}`

Intel ice driver may create multiple kworkers for one physical NIC and names those processes differently.

Example for Intel E810 card:

`pgrep -f ice-ptp | xargs -I {} sudo chrt -f --pid 75 {}`

Assigning priority needs careful consideration as assigning too high priority to any task might make system unstable.

#### SEE ALSO

[pmc(8)](/documentation/pmc/), [phc2sys(8)](/documentation/phc2sys/)