---
title: 'Start mailing list service'
prev: configure-http-server.md
up: ../install.md
next: automate-startup.md
---

Start mailing list service
==========================

  1. Ensure that RDBMS is running (on SQLite, service need not run).

  2. Ensure that MTA and HTTP server are running.

  3. Start Sympa service.

     * Systemd
       ```
       # systemctl start sympa.service
       # systemctl status sympa.service
       ```

     * initscripts or FreeBSD ports
       ```
       # service sympa start
       # service sympa status
       ```

  4. Start Web browser and access to <http://web.example.org/sympa>.

  5. Follow "``First login``" link, input a listmaster address chosen on
     [previous chapter](generate-initial-configuration.md), and then click
     "``Send my password``" button.

  6. E-mail describing how to choose your password (are you a listmaster,
     aren't you?) will be sent.  According to description, choose your
     password.

