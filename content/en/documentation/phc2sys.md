---
title: "phc2sys(8): synchronize two or more clocks"
description: "Linux PTP man page for synchronizing two or more clocks."
date: 2021-01-08 
---

### phc2sys(8): synchronize two or more clocks

#### SYNOPSIS

<code>phc2sys -a [ -r ] [ -r ] [ -f _config-file_ ] [ options ] [ _long-options_ ]</code>

<code>phc2sys [ -f _config-file_ ] [ -d _pps-device_ ] [ -s _device_ ] [ -c _device_ ] [ -O _offset_ ] [ -w ] [ options ] [ _long-options_ ] ...</code>

#### DESCRIPTION

`phc2sys` is a program which synchronizes two or more clocks in the system. Typically, it is used to synchronize the system clock to a PTP hardware clock (PHC), which itself is synchronized by the [ptp4l(8)](/documentation/ptp4l/) program.

With the `-a` option, the clocks to synchronize are fetched from the running `ptp4l` daemon and the direction of synchronization automatically follows changes of the PTP port states.

Manual configuration is also possible. When using manual configuration, two synchronization modes are supported, one uses a pulse per second (PPS) signal provided by the source clock and the other mode reads time from the source clock directly. Some clocks can be used in both modes, the mode which will synchronize the time sink with better accuracy depends on hardware and driver implementation.

#### OPTIONS

`-a`

: Read the clocks to synchronize from running `ptp4l` and follow changes in the port states, adjusting the synchronization direction automatically. The system clock (`CLOCK_REALTIME`) is not synchronized, unless the `-r` option is also specified.

`-r`

: Only valid together with the `-a` option. Instructs `phc2sys` to also synchronize the system clock (`CLOCK_REALTIME`). By default, the system clock is not considered as a possible time source. If you want the system clock to be eligible to become a time source, specify the `-r` option twice.

<code>**-f _config_**</code>

: Read configuration from the specified file. No configuration file is read by default.

<code>**-d _pps-device_**</code>

: Specify the PPS device of the source clock (e.g. `/dev/pps0`). With this option the PPS synchronization mode is used instead of the direct mode.  The matching PHC must be specified using the `-s` command line option. This option can be used only with the system clock as the time sink. Not compatible with the `-a` option.

<code>**-s _device_**</code>

: Specify the source clock by device (e.g. `/dev/ptp0`) or interface (e.g. `eth0`) or by name (e.g. `CLOCK_REALTIME` for the system clock). When this option is used
together with the `-d` option, the source clock is used only to correct the offset by whole number of seconds, which cannot be fixed with PPS alone. Not compatible with the `-a` option. This option does not support bonded interface (e.g. `bond0`, `team0`). If `ptp4l` has a port on an active-backup bond or team interface, the `-a` option can be used to track the active interface.

<code>**-i _interface_**</code>

: Performs the exact same function as `-s` for compatibility reasons. Previously enabled specifying source clock by network interface. However, this can now be done using `-s` and this option is no longer necessary. As such it has been deprecated, and should no longer be used.

<code>**-c _device_**</code>

: Specify the time sink by device (e.g. `/dev/ptp1`) or interface (e.g. `eth1`) or
by  name. The default is `CLOCK_REALTIME` (the system clock). Not compatible with the `-a` option.

<code>**-E _servo_**</code>

: Specify which clock servo should be used. Valid values are `pi` for a PI controller, `linreg` for an adaptive controller using linear regression, and `ntpshm` for the NTP SHM reference clock to allow another process to synchronize the local clock. The default is `pi`.

<code>**-P _kp_**</code>

: Specify the proportional constant of the PI controller. The default is 0.7.

<code>**-I _ki_**</code>

: Specify the integral constant of the PI controller. The default is 0.3.

<code>**-S _step_**</code>

: Specify the step threshold of the servo. It is the maximum offset that the servo corrects by changing the clock frequency instead of stepping the clock. The clock is stepped on start regardless of the option if the offset is larger than 20 microseconds (unless the `-F` option is used). It's specified in seconds. The value of 0.0 disables stepping after the start. The default is 0.0.

<code>**-F _step_**</code>

: Specify the step threshold applied only on the first update. It is the maximum offset that is corrected by changing the clock frequency. It's specified in seconds. The value of 0.0 disables stepping on start. The default is 0.00002 (20 microseconds).

<code>**-R _update-rate_**</code>

: Specify the time sink update rate when running in the direct synchronization mode. The default is 1 per second.

<code>**-N _phc-num_**</code>

: Specify the number of source clock readings used for each time sink update. Only the fastest reading is used to update the clock.  This is useful to minimize the error caused by random delays in scheduling and bus utilization. The default is 5.

<code>**-O _offset_**</code>

: Specify the offset between the sink and source times in seconds. Not compatible with the `-a` option.  See [TIME SCALE USAGE](#time-scale-usage) below.

<code>**-L _freq-limit_**</code>

: The maximum allowed frequency offset between uncorrected clock and the system monotonic clock in parts per billion (ppb). This is used as a sanity check of the synchronized clock. When a larger offset is measured, a warning message will be printed and the servo will be reset. When set to 0, the sanity check is disabled. The default is 200000000 (20%).

<code>**-M _segment_**</code>

: The number of the SHM segment used by `ntpshm` servo. The default is 0.

<code>**-u _summary-updates_**</code>

: Specify the number of clock updates included in summary statistics. The statistics include offset root mean square (RMS), maximum absolute offset, frequency offset mean and standard deviation, and mean of the delay in clock readings and standard deviation. The units are nanoseconds and parts per billion (ppb). If zero, the individual samples are printed instead of the statistics. The messages are printed at the `LOG_INFO` level. The default is 0 (disabled).

<code>**-w**</code>

: Wait until `ptp4l` is in a synchronized state. If the `-O` option is not used, also keep the offset between the sink and source times updated according to the `currentUtcOffset` value obtained from `ptp4l` and the direction of the clock synchronization. Not compatible with the `-a` option.

<code>**-n _domain-number_**</code>

: Specify the domain number used by `ptp4l`. The default is 0.

<code>**-x**</code>

: When a leap second is announced, don't apply it in the kernel by stepping the clock, but let the servo correct the one-second offset slowly by changing the clock frequency (unless the `-S` option is used).

<code>**-z _uds-address_**</code>

: Specifies the address of the server's UNIX domain socket. The default is `/var/run/ptp4l`.

<code>**-l _print-level_**</code>

: Set the maximum `syslog` level of messages which should be printed or sent to the system logger. The default is 6 (`LOG_INFO`).

<code>**-t _message-tag_**</code>

: Specify the tag which is added to all messages printed to the standard output or system log. The default is an empty string.

<code>**-m**</code>

: Print messages to the standard output.

<code>**-q**</code>

: Don't send messages to the system logger.

<code>**-h**</code>

: Display a help message.

<code>**-v**</code>

: Prints the software version and exits.

#### LONG OPTIONS

Each and every configuration file option (see below in section [FILE OPTIONS](#file-options)) may also appear as a "long" style command line argument.  For example, the `transportSpecific` option may be set using either of these two forms:

<code>-\-transportSpecific 1</code>

<code>-\-transportSpecific=1</code>

Option values given on the command line override values in the global section of the configuration file (which, in turn overrides default values).

#### CONFIGURATION FILE

The configuration file is divided into sections. Each section starts with a line containing its name enclosed in brackets and it follows with settings. Each setting is placed on a separate line, it contains the name of the option and the value separated by whitespace characters. Empty lines and lines starting with `#` are ignored.

The global section (indicated as `[global]`) sets the program options. This is the only used option.

#### FILE OPTIONS

<code>**domainNumber**</code>

: Specify the domain number used by `phc2sys`. The default is 0. Same as option `-n` (see above).

<code>**kernel_leap**</code>

: When a leap second is announced, let the kernel apply it by stepping the clock instead of correcting the one-second offset with servo, which would correct the one-second offset slowly by changing the clock frequency (unless the `step_threshold` option is set to correct such offset by stepping). Relevant only with software time stamping. The default is 1 (enabled). Same as option `-x` (see above).

<code>**logging_level**</code>

The maximum logging level of messages which should be printed. The default is 6 (`LOG_INFO`). Same as option `-l` (see above).

<code>**message_tag**</code>

: The tag which is added to all messages printed to the standard output or system log. The default is an empty string (which cannot be set in the configuration file as the option requires an argument). Same as option `-t` (see above).

<code>**sanity_freq_limit**</code>

: The maximum allowed frequency offset between uncorrected clock and the system monotonic clock in parts per billion (ppb). This is used as a sanity check of the synchronized clock. When a larger offset is measured, a warning message will be printed and the servo will be reset. When set to 0, the sanity check is disabled. The default is 200000000 (20%). Same as option `-L` (see above).

<code>**clock_servo**</code>

: The servo which is used to synchronize the local clock. Valid values are `pi` for a PI controller, `linreg` for an adaptive controller using linear regression, `ntpshm` for the NTP SHM reference clock to allow another process to synchronize the local clock (the SHM segment number is set to the domain number), and `nullf` for a servo that always dials frequency offset zero (for use in SyncE nodes). The default is `pi`. Same as option `-E` (see above).

<code>**transportSpecific**</code>

: The transport specific field. Must be in the range 0 to 255. The default is 0.

<code>**use_syslog**</code>

: Print messages to the system log if enabled.  The default is 1 (enabled). Related to option `-q` (see above).

<code>**verbose**</code>

: Print messages to the standard output if enabled.  The default is 0 (disabled). Related to option `-m` (see above).

<code>**pi_proportional_const**</code>

: Specifies the proportional constant of the PI controller. Same as option `-P` (see above).

<code>**pi_integral_const**</code>

: Specifies the integral constant of the PI controller. Same as option `-I` (see above).

<code>**step_threshold**</code>

: Specifies the step threshold of the servo. It is the maximum offset that the servo corrects by changing the clock frequency instead of stepping the clock. The clock is stepped on start regardless of the option if the offset is larger than 20 microseconds (unless the `-F` option is used). It's  specified  in seconds. The value of 0.0 disables stepping after the start. The default is 0.0. Same as option `-S` (see above).

<code>**first_step_threshold**</code>

: Specify the step threshold applied only on the first update. It is the maximum offset that is corrected by adjusting clock. It's specified in seconds. The value of 0.0 disables stepping on start. The default is 0.00002 (20 microseconds). Same as option `-F` (see above).

<code>**ntpshm_segment**</code>

: The number of the SHM segment used by `ntpshm` servo.  The default is 0. Same as option `-M` (see above).

<code>**uds_address**</code>

: Specifies the address of the server's UNIX domain socket. The default is `/var/run/ptp4`. Same as option `-z` (see above).

#### TIME SCALE USAGE

`ptp4l` uses either PTP time scale or UTC (Coordinated Universal Time) time scale.  PTP time scale is continuous and shifted against UTC by a few tens of seconds as PTP time scale does not apply leap seconds.

In hardware time stamping mode, `ptp4l` announces use of PTP time scale and PHC is used for the stamps.  That means PHC must follow PTP time scale while system clock follows UTC.  Time offset between these two is maintained by `phc2sys`.

`phc2sys` acquires the offset value either by reading it from `ptp4l` when `-a` or `-w` is in effect or from command line when `-O` is supplied.  Failure to maintain the correct offset can result in the local system clock being offset some whole number of seconds from the domain server's clock when in client mode, or incorect PTP time announced to the network in case the host is the domain server.

#### EXAMPLES

Synchronize time automatically according to the current `ptp4l` state, synchronizing the system clock to the remote server.

`phc2sys -a -r`

Same as above, but when the host becomes the domain server, synchronize time in the domain to its system clock.

`phc2sys -a -rr`

Same as above, in an IEEE 802.1AS domain.

`phc2sys -a -rr --transportSpecific=1`

The host is a domain server, PTP clock is synchronized to system clock and the time offset is obtained from `ptp4l`. `phc2sys` waits for `ptp4l` to get at least one port in server or client mode before starting the synchronization.

`phc2sys -c /dev/ptp0 -s CLOCK_REALTIME -w`

Same as above, time offset is provided on command line and `phc2sys` does not wait for `ptp4l`.

`phc2sys -c /dev/ptp0 -s CLOCK_REALTIME -O 35`

The host is in client mode, system clock is synchronized from PTP clock, `phc2sys`
waits for `ptp4l` and the offset is set automatically.

`phc2sys -s /dev/ptp0 -w`

Same as above, PTP clock id is read from the network interface, the offset is provided on command line, `phc2sys` does not wait.

`phc2sys -s eth0 -O -35`

#### WARNING

Be cautious when the same configuration file is used for both `ptp4l` and `phc2sys`. Keep in mind, that values specified in the configuration file take precedence over their default values. If a certain option, which is common to `ptp4l` and `phc2sys`, is specified to a non-default value in the configuration file (i.e., for `ptp4l`), then this non-default value applies also for `phc2sys`. This might be not what is expected.

It is recommended to use seperate configuration files for `ptp4l` and `phc2sys` in order to avoid any unexpected behavior.

#### SEE ALSO

[ptp4l(8)](/documentation/ptp4l/)