---
title: "nsm(8): NetSync Monitor client"
description: "Linux PTP man page for the NetSync Monitor client."
date: 2019-07-05
---

### nsm(8): NetSync Monitor client

#### SYNOPSIS

<code>nsm [ -f _config_ ] [ -i _interface_ ] [ _long-options_ ] [ command ] ...</code>

#### DESCRIPTION

`nsm` is a program which implements a NetSync Monitor (NSM) client. NSM is an extension to the Precision Time Protocol (PTP), which enables a client to measure the offset of its clock against any PTP clock in the network which supports NSM. It uses unicast messages, but unlike PTP in the unicast mode it does not require the server to keep any state specific to the client. It is particularly useful for monitoring.

The program reads commands from the standard input or from the command line.

#### COMMANDS

<code>**NSM _address_**</code>

: Send a NetSync Monitor request to the specified network address (IPv4 or MAC) and print the measured offset with the response.

<code>**help**</code>

: Display a help message.

#### OPTIONS

<code>**-f _config_**</code>

: Read configuration from the specified file. No configuration file is read by default.

<code>**-i _interface_**</code>

: Specify the network interface.

<code>**-h**</code>

: Display a help message.

<code>**-v**</code>

: Print the software version and exit.

#### LONG OPTIONS

Each and every configuration file option (see below in sections [PROGRAM OPTIONS](#program-options) and [PORT OPTIONS](#port-options)) may also appear as a "long" style command line argument. For example, the `transportSpecific` option may be set using either of these two forms:

<code>-\-transportSpecific 1</code>

<code>-\-transportSpecific=1</code>

Option values given on the command line override values in the global section of the configuration file (which, in turn, overrides default values).

#### CONFIGURATION FILE

The configuration file is divided into sections. Each section starts with a line containing its name enclosed in brackets and it follows with settings. Each setting is placed on a separate line, it contains the name of the option and the value separated by whitespace characters. Empty lines and lines starting with `#` are ignored.

The global section (indicated as `[global]`) sets the global program options as well as the default port specific options. Other sections are port specific sections and they override the default port options. The name of the section is the name of the configured port (e.g. `[eth0]`).

#### PORT OPTIONS

<code>**delayAsymmetry**</code>

: The time difference in nanoseconds of the transmit and receive paths. This value should be positive when the master-to-slave propagation time is longer and negative when the slave-to-master time is longer. The default is 0 nanoseconds.

<code>**network_transport**</code>

: Select the network transport. Possible values are `UDPv4` and `L2`. The default is `UDPv4`.

<code>**transportSpecific**</code>

: The transport specific field. Must be in the range 0 to 255. The default is 0.

#### PROGRAM OPTIONS

<code>**domainNumber**</code>

: The domain attribute of the local clock. The default is 0.

<code>**time_stamping**</code>

: The time stamping method. The allowed values are `hardware`, `software`, and `legacy`. The default is `hardware`.

#### WARNING

Be cautious when the same configuration file is used for both `ptp4l` and `nsm`.  Keep in mind that values specified in the configuration file take precedence over their default values. If a certain option which is common to `ptp4l` and `nsm` is specified to a non-default value in the configuration file (e.g. for `ptp4l`), then this non-default value applies also for `nsm`. This might be not what is expected.

To avoid securely this unexpected behaviour, different configuration files for `ptp4l` and `nsm` are recommended.

#### SEE ALSO

[ptp4l(8)](/documentation/ptp4l)