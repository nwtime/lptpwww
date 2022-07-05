---
title: "Default Configuration Parameters"
description: ""
---

### Default Configuration Parameters

The following tables summarize the default configuration values found in [default.cfg](https://github.com/richardcochran/linuxptp/blob/master/configs/default.cfg). Refer to [ptp4l(8)](https://github.com/richardcochran/linuxptp/blob/45f23047d22a3ab9ca0786a34d604405780cbb89/ptp4l.8) for a complete listing of all configuration options.

* * *

#### Default Program and Clock Options

| Option | Default Value | Description |
| --- | --- | --- |
| clientOnly | 0 | The local clock is a client-only clock if enabled (1).<br> The default is disabled (0).|
| clockAccuracy	 | 0xFE | Used in the PTP server selection algorithm. |
| clockClass | 248 | Denotes the traceability of the time distributed by the grandmaster clock. |
| dataset_comparison | ieee1588 | Specifies the method used when comparing data sets during the Best Master Clock Algorithm.  Possible values are `ieee1588` and`G.8275.x`. |
| domainNumber | 0 | The domain attribute of the local clock. |
| dscp_event | 0 | Defines the Differentiated Services Codepoint (DSCP) to be used for PTP event messages. Must be a value between 0 and 63. There are several media streaming standards that require specific values for this option. |
| dscp_general | 0 | Defines the Differentiated Services Codepoint (DSCP) to be used for PTP general messages. Must be a value between 0 and 63. There are several media streaming standards that recommend specific values for this option. |
| free_running | 0 | 1: Don't adjust the local clock (enabled).<br> 0: Disabled. |
| freq_est_interval | 1 | The time interval over which is estimated the ratio of the local and peer clock frequencies. It is specified as a power of two in seconds. The default is 2 seconds (1). |
| G.8275.defaultDS.localPriority | 128 | The Telecom profiles (ITU-T G.8275.1 and G.8275.2) specify an alternate Best Master Clock Algorithm with a unique data set comparison algorithm.  The value of this option is associated with the local clock and is used as a tie breaker whenever `clockClass`, `clockAccuracy`, `offsetScaledLogVariance`, and `priority2` are equal. This option is only used when `dataset_comparison` is set to `G.8275.x`. |
| maxStepsRemoved | 255 | When using this option, if the value of `stepsRemoved` of an Announce message is greater than or equal to the value of `maxStepsRemoved` the Announce message is not considered in the operation of the BMCA. |
| offsetScaledLogVariance | 0xFFFF | Characterizes the stability of the clock. |
| priority1 | 128 | The priority1 attribute of the local clock used in the PTP server selection algorithm, lower values take precedence. Must be in the range 0 to 255. |
| priority2	| 128 | The priority2 attribute of the local clock used in the PTP server selection algorithm, lower values take precedence. Must be in the range 0 to 255. |
| socket_priority | 0 | Configure the `SO_PRIORITY` of sockets to route ptp4l traffic using Linux qdiscs for traffic shaping. This option is only available with the IEEE 802.3 transport and is silently ignored when using UDP. Must be in the range of 0 to 15. |
|twoStepFlag | 1 | 1: Enable two-step mode for sync messages.<br> 0: One-step mode can be used only with hardware time stamping.|
| utc_offset | 37 | The current offset between TAI and UTC. |

* * *

#### Default Port Options

| Option | Default Value | Description |
| --- | --- | --- |
| announceReceiptTimeout | 3 | The number of missed Announce messages before the last Announce messages expires. |
| asCapable | auto | true: all the checks which can unset `asCapable` variable (as described in Section 10.2.4.1 of 802.1AS) are skipped.<br> auto: `asCapable` is initialized to false and will be set to true after the relevant checks have passed. |
| BMCA | ptp | Enables use of static roles for server and client devices instead of running the BMCA algorithm. This can speed up start time when you know the roles of the devices.<br>  noop: the traditional BMCA algorithm used by 1588 is skipped. `masterOnly` and `clientOnly` will be used to determine the role for the device.<br> ptp: runs the BMCA-related state machines. |
| delayAsymmetry | 0 | The time difference in nanoseconds of the transmit and receive paths. This value should be positive when the server-to-client propagation time is longer and negative when the client-to-server time is longer. |
| delay_response_timeout | 0 | The number of delay response messages that may go missing before triggering a synchronization fault.<br> 0: disable the delay response timeout. |
| fault_reset_interval | 4 | The time in seconds between the detection of a port's fault and the fault being reset, expressed as a power of two. The default is 16 seconds (4).<br> -128 or ASAP: reset the fault immediately.
 |
| G.8275.portDS.localPriority | 128 | The value is associated with Announce messages arriving on a particular port and is used as a tie breaker whenever `clockClass`, `clockAccuracy`, `offsetScaledLogVariance`, and `priority2` are equal. Only used when `dataset_comparison` is set to `G.8275.x`. |
| inhibit_announce | 0 | Disable the timer for announce messages and the announce message timeout timer. Used by the Automotive profile when switching over to a static BMCA.<br> 1: If this option is enabled, `ignore_source_id` has to be enabled in the client because it has no way to identify the server in the Sync and Follow_Up messages. |
| inhibit_delay_req | 0 | Don't send any delay requests.<br> 1: will need the `asCapable` option to be set to true. This is useful when running as a designated server who does not need to calculate offset from client.  |
| ignore_source_id | 0 | 1: disables source port identity checking for Sync and Follow_Up messages. This is useful when announce messages are disabled in the server and the client does not have any way to know the server's identity. |
| logAnnounceInterval | 1 | The mean time interval between Announce messages, specified as a power of two in seconds. A shorter interval makes ptp4l react faster to changes in the client/server hierarchy. The interval should be the same in the whole domain. The default is 2 seconds (1). |
| logMinDelayReqInterval | 0 | The minimum permitted mean time interval between Delay_Req messages, specified as a power of two in seconds. A shorter interval makes ptp4l react faster to the changes in the path delay. The default is 1 second (0). |
| logMinPdelayReqInterval | 0 | The minimum permitted mean time interval between `Pdelay_Req` messages, specified as a power of two in seconds. The default is 1 second (0). |
| logSyncInterval | 0 | The mean time interval between Sync messages, specified as a power of two in seconds. A shorter interval may improve accuracy of the local clock. The default is 1 second (0). |
| neighborPropDelayThresh | 20000000 | Upper limit for peer delay in nanoseconds. If the estimated peer delay is greater than this value the port is marked as not 802.1AS capable. |
| operLogPdelayReqInterval | 0 | The Pdelay Request messages interval used once the clock enters the `SERVO_LOCKED_STABLE` state.  If `msg_interval_request` is set, the local client port will adopt this rate when the local clock enters the locked stable state.  This option is specified as a power of two in seconds, and the default value is 1 second (0). |
| operLogSyncInterval | 0 | The Sync message interval to be requested once the clock enters the `SERVO_LOCKED_STABLE` state.  If `msg_interval_request` is set, the local client port will request the remote server to switch to the given message rate via a signaling message containing a Message interval request TLV.  This option is specified as a power of two in seconds, and default value is 1 second (0). |
| serverOnly | 0 | 1: prevents the port from entering the client state and the local clock will ignore Announce messages received on this port. Supports the Telecom Profiles according to ITU-T G.8265.1, G.8275.1, and G.8275.2.  |
| syncReceiptTimeout | 0 | The number of sync/follow up messages that may go missing before triggering a Best Master Clock election. Used for running in gPTP mode according to the 802.1AS-2011 standard.<br> 0: disable the sync message timeout. |

* * *

#### Default Run Time Options

| Option | Default Value | Description |
| --- | --- | --- |
| assume_two_step | 0 | 1: treat one-step responses as two-step. Used to work around buggy 802.1AS switches. |
| check_fup_sync | 0 | 1: adds an additional check using the software time stamps from the networking stack to verify the sync message arrived first. This is only useful if you do not trust the sequence IDs generated by the server. |
| clock_class_threshold	| 248 | The maximum clock class value from master, acceptible to sub-ordinate clock beyond which it moves out of lock state. |
| follow_up_info | 0 | 1: include the 802.1AS data in the Follow_Up messages. |
| hybrid_e2e | 0 | 1: hybrid delay mechanism from the draft Enterprise Profile. When enabled, ports in the client state send their delay request messages to the unicast address taken from the server's announce message. Ports in the server state will reply to unicast delay requests using unicast delay responses. This option has no effect if the `delay_mechanism` is set to P2P. |
| inhibit_multicast_service	| 0 | Some unicast mode profiles insist that no multicast message are ever transmitted.  Setting this option to 1 inhibits multicast transmission. |
| kernel_leap | 1 | When a leap second is announced, let the kernel apply it by stepping the clock instead of correcting the one-second offset with servo. Relevant only with software time stamping.  |
| logging_level | 6 | The maximum logging level of messages which should be printed. The default is LOG_INFO (6). |
| net_sync_monitor | 0 | 1: enables the NetSync Monitor (NSM) protocol which allows a station to measure how well another node is synchronized. Requires `hybrid_e2e` to also be enabled. |
| path_trace_enabled | 0 | 1: Enable the mechanism used to trace the route of the Announce messages. |
| summary_interval | 0 | The time interval, specified as a power of two in seconds, in which are printed summary statistics of the clock at the `LOG_INFO` level. The default is 1 second (0). |
| tc_spanning_tree | 0 | When running as a Transparent Clock, increment the `stepsRemoved` field of Announce messages that pass through the switch.  Enabling this option ensures that PTP message loops never form, provided the switches all implement this option together with the BMCA. |
| tx_timestamp_timeout | 10 | The number of milliseconds to poll waiting for the tx time stamp from the kernel when a message has recently been sent. |
| unicast_listen | 0 | 1: allows the port to grant unicast message contracts.  Incoming requests for will be granted limited only by the amount of memory available. |
| unicast_master_table | 0 | When set to a positive integer, specifies the table id to be used for unicast discovery. The default is 0 (unicast discovery disabled). |
| unicast_req_duration | 3600 | The service time, in seconds, requested during unicast discovery. Note that the remote node is free to grant a different duration. |
| use_syslog | 1 | 1: print messages to the system log. |
| verbose | 0 | 1: print messages to the standard output. |

* * *

#### Default Servo Options

| Option | Default Value | Description |
| --- | --- | --- |
| clock_servo | pi | The servo which is used to synchronize the local clock.<br> pi: PI controller<br> linreg: an adaptive controller using linear regression<br> ntpshm: NTP SHM reference clock to allow another process to synchronize the local clock (the SHM segment number is set to the domain number)<br> nullf: a servo that always dials frequency offset zero (for use in SyncE nodes). |
| first_step_threshold | 0.00002 | Maximum offset, specified in seconds, the servo will correct by changing the clock frequency instead of stepping the clock. This is only applied on the first update. When set to 0.0, the servo won't step the clock on start. The default is  20 microseconds (0.00002). |
| max_frequency | 900000000 | Maximum allowed frequency adjustment of the clock in parts per billion. This is an additional limit to the maximum allowed by the hardware. When set to 0, the hardware limit will be used. The default is 900000000 90% (900000000). |
| msg_interval_request | 0 | 1: triggers an adjustment to the Sync and peer delay request message intervals when the clock servo transitions into the `SERVO_LOCKED_STABLE` state.  The values to use for the new Sync and peer delay request intervals are specified by `operLogSyncInterval` and `operLogPdelayReqInterval`. |
| ntpshm_segment | 0 | he number of the SHM segment used by ntpshm servo. |
| pi_integral_const | 0.0 | The integral constant of the PI controller. |
| pi_integral_exponent | 0.4 | The ki_exponent constant in the formula used to set the integral constant of the PI controller from the sync interval. |
| pi_integral_norm_max | 0.3 | The ki_norm_max constant in the formula used to set the integral constant of the PI controller from the sync interval. |
| pi_integral_scale	| 0.0 | The ki_scale constant in the formula used to set the integral constant of the PI controller from the sync interval. |
| pi_proportional_const	| 0.0 | The proportional constant of the PI controller. |
| pi_proportional_exponent | -0.3 | The kp_exponent constant in the formula used to set the proportional constant of the PI controller from the sync interval. |
| pi_proportional_norm_max | 0.7 | The kp_norm_max constant in the formula used to set the proportional constant of the PI controller from the sync interval. |
| pi_proportional_scale	| 0.0 | The kp_scale constant in the formula used to set the proportional constant of the PI controller from the sync interval. |
| sanity_freq_limit	| 200000000 | The maximum allowed frequency offset between uncorrected clock and the system monotonic clock in parts per billion. This is used as a sanity check of the synchronized clock. When a larger offset is measured, a warning message will be printed and the servo will be reset. When set to 0, the sanity check is disabled. The default is  20% (200000000). |
| servo_num_offset_values | 10 | The number of offset values considered in order to transition from the `SERVO_LOCKED` to the `SERVO_LOCKED_STABLE` state. The transition occurs once the last `servo_num_offset_values` offsets are all below the `servo_offset_threshold` value. |
| servo_offset_threshold | 0 | The offset threshold used to transition from `SERVO_LOCKED` to `SERVO_LOCKED_STABLE`.  The transition occurs once the last `servo_num_offset_values` offsets are all below the threshold value. The default is disabled (0). |
| step_threshold | 0.0 | The maximum offset, in seconds, the servo will correct by changing the clock frequency instead of stepping the clock. 0.0: the servo will never step the clock except on start. |
| write_phase_mode | 0 | 1: enables the write phase feature of a PTP Hardware Clock.  If supported by the device, this mode uses the hardware's built in phase offset control instead of frequency offset control. |

* * *

#### Default Transport Options

| Option | Default Value | Description |
| --- | --- | --- |
| p2p_dst_mac | 01:80:C2:00:00:0E | The MAC address to which peer delay messages should be sent. Relevant only with L2 transport. |
| ptp_dst_mac | 01:1B:19:00:00:00 | The MAC address to which PTP messages should be sent. Relevant only with L2 transport. |
| transportSpecific	| 0x0 | The transport specific field. Must be in the range 0 to 255. |
| udp_ttl | 1 | Specifies the TTL value for IPv4 multicast messages and the hop limit for IPv6 multicast messages. Only relevant with IPv4/IPv6 UDP transports. The default (1) restricts messages sent by ptp4l to the same subnet. |
| udp6_scope | 0x0E | Specifies the scope for IPv6 multicast messages (RFC 4291). The default (0x0E) is for the global scope. |
| uds_address | /var/run/ptp4l | Address of the UNIX domain socket for receiving local management messages. |
| uds_file_mode | 0660 | File mode of the UNIX domain socket used for receiving local management messages, specified as an octal number. |
| uds_ro_address | /var/run/ptp4lro | Address of the second UNIX domain socket for receiving local management messages, which is restricted to GET actions and does not forward messages to other ports. Access to this socket can be given to untrusted applications for monitoring purposes. |
| uds_ro_file_mode | 0666 | File mode of the second (read-only) UNIX domain socket used for receiving local management messages, specified as an octal number. |

* * *

#### Default Interface Options

| Option | Default Value | Description |
| --- | --- | --- |
| boundary_clock_jbod | 0 | 1: allows ptp4l to work as a boundary clock using "just a bunch of devices" that are not synchronized to each other. The collection of clocks must be synchronized by an external program, for example phc2sys(8) in automatic mode. |
| clock_type | OC | Specifies the kind of PTP clock.<br> OC: ordinary clock<br> BC: boundary clock<br> P2P_TC: peer to peer transparent clock<br> E2E_TC: end to end transparent clock.  A multi-port ordinary clock will automatically be configured as a boundary clock. |
| delay_filter | moving_median | Algorithm used to filter the measured delay and peer delay. Possible values are `moving_average` and `moving_median`. |
| delay_filter_length | 10 | Length of the delay filter in samples. |
| delay_mechanism | E2E | Delay mechanism. Possible values are `E2E`, `P2P`, and `Auto`. |
| egressLatency	 | 0 | Difference in nanoseconds between the actual transmission time at the reference plane and the reported transmit time stamp. This value is added to egress time stamps obtained from the hardware. |
| ingressLatency | 0 | Difference in nanoseconds between the reported receive time stamp and the actual reception time at reference plane. This value is subtracted from ingress time stamps obtained from the hardware. |
| network_transport	| UDPv4 | Possible values are `UDPv4`, `UDPv6`, and `L2`. |
| phc_index	 | -1 | Index of the PHC used for synchronization with hardware timestamping. Useful with virtual clocks running on top of a free-running physical clock.<br> -1: index is set to the PHC associated with the interface or the device specified by the `-p` option. |
| time_stamping	 | hardware | Time stamping method.  Allowed values are `hardware`, `software`, `legacy`, `onestep`, and `p2p1step`. |
| tsproc_mode | filter | Time stamp processing mode used to calculate offset and delay. Possible values are `filter`, `raw`, `filter_weight`, and `raw_weight`. Raw modes perform well when the rate of sync messages is similar to the rate of delay messages. Weighting is useful with larger network jitters. |

* * *

#### Default Clock Description

| Option | Default Value | Description |
| --- | --- | --- |
| productDescription | ;; | Product description string. Allowed values must be of the form `manufacturerName;modelNumber;instanceIdentifier` and contain at most 64 utf8 symbols. |
| revisionData | ;; | Revision description string which contains the revisions for node hardware (HW), firmware (FW), and software (SW). Allowed values are of the form `HW;FW;SW` and contain at most 32 utf8 symbols. |
| manufacturerIdentity | 00:00:00 | The manufacturer id which should be an OUI owned by the manufacturer. |
| userDescription | ; | User description string. Allowed values are of the form `name;location` and contain at most 128 utf8 symbols. |
| timeSource | 0xA0 | A single byte code representing the kind of local clock in use. The value is purely informational, having no effect on the outcome of the Best Master Clock algorithm, and is advertised when the clock becomes grand master. |