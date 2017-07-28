# NAME 

bulk, bulk.pl - Daemon for submitting messages to SMTP engine

# SYNOPSIS

**bulk.pl** \[ **--foreground** \] \[ **--debug** \]

# DESCRIPTION 

This daemon must be run along with sympa\_msg.pl(8).  It regularly checks the
content of outgoing (bulk) spool and submit the messages it finds in it to the
sendmail engine.  Several daemons may be used on deferent server for huge
traffic.

# OPTIONS

- **-d**, **--debug**

    Sets the debug mode

- **-f**, **--config=**_file_

    Force bulk to use an alternative configuration file instead
    of `/etc/sympa/sympa.conf`.

- **-F**, **--foreground**

    Prevents the script from being daemonized

- **-h**, **--help**

    Prints this help message.

- **--log\_level=**_level_

    Set log level.

- **-m**, **--mail**

    Logs every sendmail calls.

# FILES

`/home/sympa/bulk.pid` this file contains the process IDs
of `bulk.pl`.

# SEE ALSO

[sympa.conf(5)](./sympa.conf.5.md), [sympa\_msg(8)](./sympa_msg.8.md).

[Sympa::Spindle::ProcessOutgoing](./Sympa::Spindle::ProcessOutgoing.3.md).

# HISTORY

bulk.pl initially written by Serge Aumont appeared on Sympa 6.0.
