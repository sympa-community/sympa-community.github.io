---
title: 'Configure system log'
prev: setup-database.md
up: ../install.md
next: configure-mail-server.md
---

Configure system log
====================

Sympa reports events and errors using syslog protocol.

Sympa configuration parameters
------------------------------

  * The log facility is ``LOCAL1`` by default, and it may be changed by
    [``syslog``](/gpldoc/man/sympa_config.5.html#syslog) parameter in
    [``sympa.conf``](../layout.md#config).

  * The default value of
    [``log_socket_type``](/gpldoc/man/sympa_config.5.html#log_socket_type) parameter
    is ``unix``, using Unix domain socket to communicate with syslog
    server.  Some platforms including Solaris prefer to ``stream``.  The value
    may also be ``inet``, using TCP or UDP connection.

Instruction by syslog servers
-----------------------------

### Traditional syslog

  1. Create log file:
     ```bash
     # touch /var/log/sympa.log
     # chmod 640 /var/log/sympa.log
     ```

  2. Add following line to syslog.conf:
     ```
     local1.* -/var/log/sympa.log
     ```

  3. Reload syslog service.

Tests
-----

  1. Run [``testlogs``](/gpldoc/man/testlogs.1.html) utility (Note: replace
     [``$SCRIPTDIR``](../layout.md#scriptdir) below):
     ```bash
     # $SCRIPTDIR/testlogs.pl
     ```
     And confirm that following message will be shown:
     ```
     Ok, now check logs
     ```

  2. Check log file and confirm that following log line was recorded:
     ```
     sympa/testlogs[XXXX]: info main:: Logs seems OK, default log level 0
     ```

If something went unexpected, check configuration of syslog server.

