---
title: "timemaster(8): run NTP with PTP as reference clocks"
description: "Linux PTP man page for timemaster, a program used to synchronize the system clock to NTP and PTP time sources."
date: 2022-02-06 
---

### timemaster(8): run NTP with PTP as reference clocks

#### SYNOPSIS

<code>timemaster [ -nmqv ] [ -l _print-level_ ] -f _file_</code>

#### DESCRIPTION

`timemaster` is a program that uses `ptp4l` and `phc2sys` in combination with `chronyd` or `ntpd` to synchronize the system clock to NTP and PTP time sources. The PTP time is provided by `phc2sys` and `ptp4l` via SOCK reference clock to `chronyd` or SHM reference clock to `ntpd`, which can compare all time sources and use the best sources to synchronize the system clock.

On start, `timemaster` reads a configuration file that specifies the NTP and PTP time sources, checks which network interfaces have and share a PTP hardware clock (PHC), generates configuration files for `ptp4l` and `chronyd`/`ntpd`, and starts the `ptp4l`, `phc2sys`, and `chronyd`/`ntpd` processes as needed. Then, it waits for a signal to kill the processes, removes the generated configuration files, and exits.

#### OPTIONS

<code>**-f _file_**</code>

: Specify the path to the `timemaster` configuration file.

<code>**-n**</code>

: Don't start the programs, only print their configuration files and the commands that would be executed if this option wasn't specified.

<code>**-l _level_**</code>

: Set the maximum `syslog` level of messages which should be printed or sent to the system logger. The default value is 6 (`LOG_INFO`).

<code>**-m**</code>

: Print messages to the standard output.

<code>**-q**</code>

: Don't send messages to the system logger.

<code>**-v**</code>

: Print the software version and exit.

<code>**-h**</code>

: Display a help message and exit.

#### CONFIGURATION FILE

The configuration file is divided into sections. Each section starts with a line containing its name enclosed in brackets and it follows with settings. Each setting is placed on a separate line, it contains the name of the option and the value separated by whitespace characters. Empty lines and lines starting with `#` are ignored.

Sections that can used in the configuration file and options that can be set in them are described below.

##### [timemaster]

<code>**ntp_program**</code>

: Select which NTP implementation should be used. Possible values are `chronyd` and `ntpd`. The default value is `chronyd`. Limitations of the implementations relevant to the timemaster configuration are listed in [NOTES](#notes).

<code>**rundir**</code>

: Specify the directory where the `chronyd`, `ntpd` and `ptp4l` configuration files and sockets should be generated. The directory will be created if it doesn't exist. The default value is `/var/run/timemaster`.

<code>**first_shm_segment**</code>

: Specify the first number in a sequence of SHM segments that will be used by `ptp4l` and `phc2sys`. The default value is 0. Increasing the number can be useful to avoid conflicts with time sources that are not started by `timemaster`, e.g. `gpsd` using segments number 0 and 1.

<code>**restart_processes**</code>

: Enable or disable restarting of processes started by `timemaster`. If the option is set to a non-zero value, all processes except `chronyd` and `ntpd` will be automatically restarted when terminated and `timemaster` is running for at least one second (i.e. the process did not terminate due to a configuration error). If a process was terminated and is not started again, `timemaster` will kill the other processes and exit with a non-zero status. The default value is 1 (enabled).

<code>**use_vclocks**</code>

: Enable or disable synchronization with virtual clocks. If enabled, `timemaster` will create virtual clocks running on top of physical clocks needed by configured PTP domains. This enables hardware time stamping for multiple `ptp4l` instances using the same network interface. The default value is -1, which enables the virtual clocks if running on Linux 5.18 or later.

##### [ntp_server address]

The `ntp_server` section specifies an NTP server that should be used as a time source. The address of the server is included in the name of the section.

<code>**minpoll**</code>
<code>**maxpoll**</code>

: Specify the minimum and maximum NTP polling interval as powers of two in seconds. The default values are 6 (64 seconds) and 10 (1024 seconds) respectively. Shorter polling intervals usually improve the accuracy significantly, but they should be used only when allowed by the operators of the NTP service (public NTP servers generally don't allow too frequent queries). If the NTP server is located on the same LAN, polling intervals around 4 (16 seconds) might give best accuracy.

<code>**iburst**</code>

: Enable or disable sending a burst of NTP packets on start to speed up the initial synchronization. Possible values are 1 and 0. The default value is 0 (disabled).

<code>**ntp_options**</code>

: Specify extra options that should be added for this source to the `server` directive in the configuration file of the selected NTP implementation. No extra options are added by default.

##### [ptp_domain number]

The `ptp_domain` section specifies a PTP domain that should be used as a time source. The PTP domain number is included in the name of the section. The `ptp4l` instances are configured to run in the `clientOnly` mode. In this section at least the `interfaces` option needs to be set, other options are optional.

<code>**interfaces**</code>

: Specify which network interfaces should be used for this PTP domain. A separate `ptp4l` instance will be started for each group of interfaces sharing the same PHC and for each interface that supports only SW time stamping. HW time stamping is enabled automatically. If an interface with HW time stamping is specified also in other PTP domains and virtual clocks are disabled, only the `ptp4l` instance from the first PTP domain will be using HW time stamping.

<code>**ntp_poll**</code>

: Specify the polling interval of the SOCK/SHM reference clock reading samples from `ptp4l` or `phc2sys`. It's specified as a power of two in seconds. The default value is 2 (4 seconds).

<code>**phc2sys_poll**</code>

: Specify the polling interval used by `phc2sys` to read a PTP clock synchronized by `ptp4l` and update the SOCK/SHM sample for `chronyd`/`ntpd`. It's specified as a power of two in seconds. The default value is 0 (1 second).

<code>**delay**</code>

: Specify the maximum assumed roundtrip delay to the primary source of the time in this PTP domain. This value is included in the distance used by `chronyd` in the source selection algorithm to detect falsetickers and assign weights for source combining. The default value is 1e-4 (100 microseconds). With `ntpd`, the `tos mindist` command can be used to set a limit with similar purpose globally for all time sources.

<code>**ntp_options**</code>

: Specify extra options that should be added for this source to the `refclock` or `server` directives in the configuration file of the selected NTP implementation. No extra options are added by default.

<code>**ptp4l_option**</code>

: Specify an extra `ptp4l` option specific to this PTP domain that should be added to the configuration files generated for `ptp4l`. This option may be used multiple times in one `ptp_domain` section.

##### [chronyd]

<code>**path**</code>

: Specify the path to the `chronyd` binary. The default value is `chronyd` to search for the binary in `PATH`.

<code>**options**</code>

: Specify extra options that should be added to the `chronyd` command line. No extra options are added by default.

##### [chrony.conf]

Settings specified in this section are copied directly to the configuration file generated for `chronyd`. If this section is not present in the `timemaster` configuration file, the following setting will be added:

&emsp;&emsp; `makestep 1 3`

This configures `chronyd` to step the system clock in the first three updates if the offset is larger than 1 second.

##### [ntpd]

<code>**path**</code>

: Specify the path to the `ntpd` binary. The default value is `ntpd` to search for the binary in `PATH`.

<code>**options**</code>

: Specify extra options that should be added to the `ntpd` command line. No extra options are added by default.

##### [ntp.conf]

Settings specified in this section are copied directly to the configuration file generated for `ntpd`. If this section is not present in the `timemaster` configuration file, the following settings will be added:

<pre>
restrict default nomodify notrap nopeer noquery
restrict 127.0.0.1
restrict ::1
</pre>

This configures `ntpd` to use safe default restrictions.

##### [phc2sys]

<code>**path**</code>

: Specify the path to the `phc2sys` binary. The default value is `phc2sys` to search for the binary in `PATH`.

<code>**options**</code>

: Specify extra options that should be added to all `phc2sys` command lines. By default, `-l 5` is added to the command lines.

##### [ptp4l]

<code>**path**</code>

: Specify the path to the `ptp4l` binary. The default value is `ptp4l` to search for the binary in `PATH`.

<code>**options**</code>

" Specify extra options that should be added to all `ptp4l` command lines. By default, `-l 5` is added to the command lines.

##### [ptp4l.conf]

Settings specified in this section are copied directly to the global section of the configuration files generated for all `ptp4l` instances. There is no default content of this section.

Other sections (e.g. `[unicast_master_table]`) may be specified here, but lines beginning with the bracket need to be prefixed with the `>` character to prevent `timemaster` from parsing it as a beginning of another section.

#### NOTES
For best accuracy, `chronyd` is usually preferred over `ntpd`, it also synchronizes the system clock faster. Both NTP implementations, however, have some limitations that need to be considered before choosing the one to be used in a given `timemaster` configuration.

The `chronyd` limitations are:

* In version 1.31 and older, the maximum number of reference clocks used at the same time is 8. This limits the number of PHCs and interfaces using SW time stamping that can be used for PTP.

* Using polling intervals (`minpoll`, `maxpoll`, `ntp_poll` options) shorter than 2 (4 seconds) is not recommended with versions before 1.30. With 1.30 and later values of 0 or 1 can be used for NTP sources and negative values for PTP sources (`ntp_poll`) to specify a subsecond interval.

The `ntpd` limitations are:

* In versions before 4.2.8p1, only the first two shared-memory segments created by the `ntpd` SHM refclock driver have owner-only access. Other segments are created with world access, which allows any user on the system to write to the segments and disrupt or take control over the synchronization of the clock. In 4.2.8p1 the access was made configurable with the mode option, which is set by `timemaster` for owner-ownly access.

* The shortest polling interval for all sources is 3 (8 seconds).

* Nanosecond resolution in the SHM refclock driver is supported in version 4.2.7p303 and later, older versions have only microsecond resolution.

#### EXAMPLES

A minimal configuration file using one NTP source and two PTP sources would be:

<pre>
[ntp_server 10.1.1.1]

[ptp_domain 0]
interfaces eth0

[ptp_domain 1]
interfaces eth1
</pre>

A more complex example using all `timemaster` options would be:

<pre>
[ntp_server 10.1.1.1]
minpoll 3
maxpoll 4
iburst 1
ntp_options key 12

[ptp_domain 0]
interfaces eth0 eth1
ntp_poll 0
phc2sys_poll -2
delay 10e-6
ntp_options prefer
ptp4l_option clock_servo linreg
ptp4l_option delay_mechanism P2P

[timemaster]
ntp_program chronyd
rundir /var/run/timemaster
first_shm_segment 1
restart_processes 0
use_vclocks 0

[chronyd]
path /usr/sbin/chronyd
options

[chrony.conf]
makestep 1 3
logchange 0.5
rtcsync
driftfile /var/lib/chrony/drift

[ntpd]
path /usr/sbin/ntpd
options -u ntp:ntp

[ntp.conf]
restrict default nomodify notrap nopeer noquery
restrict 127.0.0.1
restrict ::1
driftfile /var/lib/ntp/drift

[phc2sys]
path /usr/sbin/phc2sys
options -l 5

[ptp4l]
path /usr/sbin/ptp4l
options

[ptp4l.conf]
logging_level 5
</pre>

#### SEE ALSO

[chronyd(8)](https://chrony.tuxfamily.org/doc/3.5/chronyd.html), [ntpd(8)](https://doc.ntp.org/current-stable/ntpd/), [phc2sys(8)](/documentation/phc2sys/), [ptp4l(8)](/documentation/ptp4l/)