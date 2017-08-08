---
title: 'Managing services'
prev: ../upgrade.md
up: ../admin.md
---

Managing services
=================

Services overview
-----------------

### Sympa services

Several daemons collaboratively provide mailing list service.

* [``archived.pl``](../man/archived.8.md)

  Archiving daemon.

* [``bounced.pl``](../man/bounced.8.md)

  Bounce processing daemon.

* [``bulk.pl``](../man/bulk.8.md)

  Daemon for submitting messages to mail transfer agent (MTA).
  It may fork several workers as necessity.

* [``sympa_msg.pl``](../man/sympa_msg.8.md)

  Daemon to handle incoming messages.
  It may fork several workers as necessity.

* [``task_manager.pl``](../man/task_manager.8.md)

  Daemon to process periodical tasks.

And optionally:

* [``sympa_automatic.pl``](../man/sympa_automatic.8.md)

  Automatic list creation daemon.
  It may be invoked by ``sympa_msg.pl`` as necessity.

### Mail transfer agent (MTA)

Mail transfer agent (MTA) injects incoming messages into incoming spool of
Sympa, and takes outgoing messages from Sympa up to distribute them beyond
Internet.

### HTTP server and WWSympa

HTTP server provides users web interface on behalf of WWSympa
([``wwsympa.fcgi``](../man/wwsympa.8.md)), the FastCGI server.

WWSympa may either be invoked by HTTP server or launch as an independent
daemon.

### Database server

Database server provides backend database for services above.

Starting and stopping services
------------------------------

### Checking status of Sympa services

* Systemd:
  ```bash
  # systemctl status sympa
  ```

* initscripts:
  ```bash
  # service sympa status
  ```

### Stopping services

1. Ensure that HTTP server is stopping.

   On nginx, only WWSympa FastCGI service (``wwsympa``) may be stopped
   in many cases: nginx itself may not be stopped.

2. Ensure that mail server is stopping.

3. Stop Sympa services.

   * Systemd:
     ```bash
     # systemctl stop sympa
     # systemctl status sympa
     ```

   * initscripts:
     ```bash
     # service sympa stop
     # service sympa status
     ```

4. Database service may be stopped when all services above are stopping.

### Starting services

1. Ensure that database service is running.

2. Start Sympa services.

   * Systemd:
     ```bash
     # systemctl start sympa
     # systemctl status sympa
     ```

   * initscripts:
     ```bash
     # service sympa start
     # service sympa status
     ```

3. Start mail server.

4. Start HTTP server, if necessary.

   On nginx, WWSympa FastCGI service (``wwsympa``) must be started
   along with nginx itself.

