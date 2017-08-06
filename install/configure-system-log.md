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
  [``syslog``](../man/sympa.conf.5.md#syslog) parameter in
  [``sympa.conf``](../layout.md#config).

* The default value of
  [``log_socket_type``](../man/sympa.conf.5.md#log_socket_type) parameter is
  ``unix``, using Unix domain socket to communicate with syslog server.  Some
  platforms including Solaris prefer to ``stream``.  The value may also be
  ``inet``, using TCP or UDP connection.

Traditional syslog
------------------

1. Create log file:
   ```bash
   # touch /var/log/syslog
   # chmod 640 /var/log/syslog
   ```

2. Add following line to syslog.conf:
   ```
   local1.* /var/log/sympa.log
   ```

3. Reload syslog service.

