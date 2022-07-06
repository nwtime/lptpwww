---
title: "ts2phc(8): Synchronizes one or more PTP Hardware Clocks using external time stamps"
description: "Man page for ts2phc which synchronizes PTP Hardware Clocks to external time stamp signals."
date: 2021-07-18 
---

### ts2phc(8): Synchronizes one or more PTP Hardware Clocks using external time stamps

#### SYNOPSIS

<code>ts2phc [ -hmqv ] [ -c _device|name_ ] [ -f _config_ ] [ -l _print-level_ ] [ -s _device|name_ ] [ _long-options_ ] ...</code>

#### DESCRIPTION

`ts2phc` synchronizes PTP Hardware Clocks (PHC) to external time stamp signals. A single source may be used to distribute time to one or more PHC devices.

#### OPTIONS

<code>**-c _device|name_)**</code>

: Specifies a PHC to be synchronized. The clock may be identified by its character device (like `/dev/ptp0`) or its associated network interface (like `eth0`). This option may be given multiple times.

<code>**-f _config_**</code>

: Read configuration from the specified file. No configuration file is read by default.

<code>**-h**</code>

: Displays the command line help summary.

<code>**-l _print-level_**</code>

: Sets the maximum `syslog` level of messages which should be printed or sent to the system logger. The default is 6 (`LOG_INFO`).

<code>**-m**</code>

: Prints log messages to the standard output.

<code>**-q**</code>

: Prevents sending log messages to the system logger.

<code>**-s _device|name_**</code>

: Specifies the source of the PPS signal. Use the key word `generic` for an external 1-PPS without ToD information. When using a PHC as the time source, the clock may be identified by its character device (like `/dev/ptp0`) or its associated network interface (like `eth0`). Use the key word `nmea` for an external 1-PPS from a GPS providing ToD information via the RMC NMEA sentence.

<code>**-v**</code>

: Prints the software version and exits.

#### LONG OPTIONS

Each and every configuration file option (see below) may also appear as a "long" style command line argument.  For example, the `use_syslog` option may be set using either of these two forms.

`--use_syslog 1`

`--use_syslog=1`

Option values given on the command line override values in the global section of the configuration file.

#### CONFIGURATION FILE

The configuration file is divided into sections. Each section starts with a line containing its name enclosed in brackets and it follows with settings. Each setting is placed on a separate line, it contains the name of the option and the value separated by whitespace characters. Empty lines and lines starting with `#` are ignored.

There are two different section types.

1. The global section (indicated as `[global]`) sets the program options and the default time sink options. Other sections are clock specific sections, and they override the default options.

2. Time sink sections give the name of the configured PHC (e.g. `[eth0]`). Time sinks specified in the configuration file need not be specified with the `-c` command line option.

#### GLOBAL OPTIONS

<code>**first_step_threshold**</code>

: The maximum offset, specified in seconds, that the servo will correct by changing the clock frequency instead of stepping the clock. This is only applied on the first update. When set to 0.0, the servo will not step the clock on start. The default is 0.00002 (20 microseconds).

<code>**free_running**</code>

: When set to 1, no PHC will be adjusted. This option can be useful in test scenarios, for example to determine how well synchronized a group of local clocks are to each other. The default is 0 (adjust the clocks).

<code>**leapfile**</code>

: The path to the current leap seconds definition file. In a Debian system this file is provided by the `tzdata` package and can be found at `/usr/share/zoneinfo/leap-seconds.list`. If a leapfile is configured it will be reloaded if modified. The default is an empty string, which causes the program to use a hard coded table that reflects the known leap seconds on the date of the software's release.

<code>**logging_level**</code>

: The maximum logging level of messages which should be printed. The default is 6 (`LOG_INFO`).

<code>**max_frequency**</code>

: The maximum allowed frequency adjustment of the clock in parts per billion.  This is an additional limit to the maximum allowed by the hardware. When set to 0, the hardware limit will be used. The default is 900000000 (90%).

<code>**message_tag**</code>

: The tag which is added to all messages printed to the standard output or system log.  The default is an empty string (which cannot be set in the configuration file as the option requires an argument).

<code>**step_threshold**</code>

: The maximum offset, specified in seconds, that the servo will correct by changing the clock frequency instead of stepping the clock. When set to 0.0, the servo will never step the clock except on start. The default is 0.0.

<code>**ts2phc.nmea_remote_host, ts2phc.nmea_remote_port**</code>

: Specifies the remote host providing ToD information when using the `nmea` PPS signal source.  Note that if these two options are both specified, then the given remote connection will be used in preference to the configured serial port. These options default to the empty string, that is, not specified.

<code>**ts2phc.nmea_serialport, ts2phc.nmea_baudrate**</code>

: Specifies the serial port and baudrate in bps for character device providing ToD information when using the `nmea` PPS signal source. Note that if the options, `ts2phc.nmea_remote_host` and `ts2phc.nmea_remote_port`, are both specified, then the given remote connection will be used in preference to the configured serial port. The default serial port is `/dev/ttyS0`. The default baudrate is 9600 bps.

<code>**ts2phc.pulsewidth**</code>

: The expected pulse width of the external PPS signal in nanoseconds. When `ts2phc.extts_polarity` is `both`, the given pulse width is used to detect and discard the time stamp of the unwanted edge. The supported range is 1000000 to 990000000 nanoseconds. The default is 500000000 nanoseconds.

<code>**use_syslog**</code>

: Print messages to the system log if enabled.  The default is 1 (enabled).

<code>**verbose**</code>

: Print messages to the standard output if enabled.  The default is 0 (disabled).

#### TIME SINK OPTIONS

<code>**ts2phc.channel**</code>

: The external time stamping or periodic output channel to be used. Some PHC devices feature programmable pins and one or more time stamping channels.  This option allows selecting a particular channel to be used.  When using a PHC device as the PPS source, this option selects the periodic output channel. The default is channel 0.

<code>**ts2phc.extts_correction**</code>

: The value, in nanoseconds, to be added to each PPS time stamp. The default is 0 (no correction).

<code>**ts2phc.extts_polarity**</code>

The polarity of the external PPS signal, either `rising` or `falling`. Some PHC devices always time stamp both edges.  Setting this option to `both` will allow the `ts2phc` program to work with such devices by detecting and ignoring the unwanted edge.  In this case be sure to set `ts2phc.pulsewidth` to the correct value. The default is `rising`.

<code>**ts2phc.master**</code>

: Setting this option to 1 configures the given PHC device as the source of the PPS signal. The default is 0 for the time sink role.

<code>**ts2phc.pin_index**</code>

: The pin index to be used. Some PHC devices feature programmable pins, and this option allows configuration of a particular pin for the external time stamping or periodic output function. The default is pin index 0.

#### WARNING

Be cautious when sharing the same configuration file between `ptp4l`, `phc2sys`, and `ts2phc`.  Keep in mind that values specified in the configuration file take precedence over the default values.  If an option which is common to the other programs is set in the configuration file, then the value will be applied to all the programs using the file, and this might not be what is expected.

It is recommended to use separate configuration files for `ptp4l`, `phc2sys`, and `ts2phc` in order to avoid any unexpected behavior.

#### SEE ALSO
[phc2sys(8)](/documentation/phc2sys/), [ptp4l(8)](/documentation/ptp4l/)