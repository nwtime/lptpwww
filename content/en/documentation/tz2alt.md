---
title: "tz2alt(8): Monitors daylight savings time changes and publishes them to PTP stack."
description: "Man page for tz2alt which monitors daylight savings time changes and publishes them to PTP stack."
date: 2023-02-25 
---

#### tz2alt(8): Monitors daylight savings time changes and publishes them to PTP stack

#### SYNOPSIS

<code>ts2alt [ -hmqv ] [ -f _conf_ ] [ -k _key_ ] [ -p _period_ ] [ -w _window_ ] [ -z _timezone_ ] [ _long-options_ ] ...</code>
 
#### DESCRIPTION

`tz2alt` leverages the local time zone database to monitor for changes in daylight savings time and publishes the pending changes to the PTP
stack.

#### OPTIONS

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

<code>**-v**</code>

: Prints the software version and exits.

#### LONG OPTIONS

Each and every configuration file option (see below) may also appear as a "long" style command line argument.  For example, the `use_syslog`
option may be set using either of these two forms.

<code>-\-use_syslog 1</code>

<code>\-\-use_syslog=1</code>

Option values given on the command line override values in the global section of the configuration file.

#### CONFIGURATION FILE

The configuration file is divided into sections. Each section starts with a line containing its name enclosed in brackets and it follows with settings. Each setting is placed on a separate line, it contains the name of the option and the value separated by whitespace characters. Empty lines and lines starting with `#` are ignored.

#### GLOBAL OPTIONS

<code>**domainNumber**</code>

: The domain attribute of the local clock. The default is 0.

<code>**leapfile**</code>

: The path to the current leap seconds definition file. In a Debian system this file is provided by the `tzdata` package and can be found at
`/usr/share/zoneinfo/leap-seconds.list`. If a leapfile is configured it will be reloaded if modified. The default is an empty string, which
causes the program to use a hard coded table that reflects the known leap seconds on the date of the software's release.

<code>**logging_level**</code>

: The maximum logging level of messages which should be printed. The default is 6 (`LOG_INFO`).

<code>**message_tag**</code>

: The tag which is added to all messages printed to the standard output or system log. If the tag contains the string `"{level}"`, it will be replaced
with the log level of the message as a number.  The default is an empty string (which cannot be set in the configuration file as the option requires an argument).

<code>**transportSpecific**</code>

: The transport specific field. Must be in the range 0 to 255. The default is 0.

<code>**use_syslog**</code>

: Print messages to the system log if enabled.  The default is 1 (enabled).

<code>**verbose**</code>

: Print messages to the standard output if enabled.  The default is 0 (disabled).

#### WARNING

Be cautious when sharing the same configuration file between `ptp4l`, `phc2sys`, and `tz2alt`.  Keep in mind that values specified in the
configuration file take precedence over the default values.  If an option which is common to the other programs is set in the configuration file, then the value will be applied to all the programs using the file, and this might not be what is expected.

It is recommended to use separate configuration files for `ptp4l`, `phc2sys`, and `tz2alt` in order to avoid any unexpected behavior.

#### SEE ALSO

[ptp4l(8)](/documentation/ptp4l/)