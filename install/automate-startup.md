---
title: 'Automate startup'
prev: start-mailing-list-service.md
up: ../install.md
next: ../upgrade.md
---

Automate startup
================

Systemd
-------

1. Units should be customized to ensure Sympa starts after database server
   (if you run it except SQLite on the same host as Sympa).  It can be done
   by copying and editing a configuration file:
   [``/etc/systemd/system/sympa.service.d/dependencies.conf``](../examples/systemd/dependencies.conf)

   Note that database service should also start before HTTP service.

2. Some paths may be placed under volatile directory, for example PID
   directory ``/var/run/sympa`` under ``/var/run``.  They have to be recreated
   at the next boot-up time.  If it is the case, create
   ``/usr/lib/tmpfiles.d/sympa.conf`` with the content:
   ```
   d /var/run/sympa 0755 sympa sympa -
   ```

3. Activate Sympa service.
   ```bash
   # systemctl enable sympa.service
   ```

You should activate other services also: Mail transfer agent (MTA) and
HTTP server.

initscripts
-----------

1. Activate Sympa service.

   ```bash
   # chkconfig sympa on
   ```

You should activate other services also:
Database server, mail transfer agent (MTA) and HTTP server.

