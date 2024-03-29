---
title: snmpd.conf
description: "Example snmpd.conf configuration for use with Linux PTP."
date: 2018-08-21
---

### snmpd.conf

Linux PTP parameters for `snmpd.conf`.

<pre>
# Map 'linuxptp' community to the 'LinuxPtpUser'
# Map 'public' community to the 'AllUser'
#       sec.name        source          community
com2sec LinuxPtpUser    default         linuxptp
com2sec AllUser         default         public

# Map 'LinuxPtpUser' to 'LinuxPtpGroup' for SNMP Version 2c
# Map 'AllUser' to 'AllGroup' for SNMP Version 2c
#                       sec.model       sec.name
group   LinuxPtpGroup   v2c             LinuxPtpUser
group   AllGroup        v2c             AllUser

# Define 'SystemView', which includes everything under .1.3.6.1.2.1.241
# Define 'AllView', which includes everything under .1
#                       incl/excl       subtree
view    SystemView      included        .1.3.6.1.2.1.241
view    AllView         included        .1

# Give 'ConfigGroup' read access to objects in the view 'SystemView'
# Give 'AllGroup' read access to objects in the view 'AllView'
#                       context model   level   prefix  read            write   notify
access  LinuxPtpGroup   ""      any     noauth  exact   SystemView      none    none
access  AllGroup        ""      any     noauth  exact   AllView         none    none

# turn on the AgentX master agent support
master agentx
<pre>