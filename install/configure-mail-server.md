---
title: 'Configure mail server'
priv: configure-system-log.md
up: ../install.md
next: configure-http-server.md
---

Configure mail server
=====================

Requirements
------------

  * Mail transfer agent (MTA):
    See also "[Requirements](../requirements.md#mail-transfer-agent-mta)".

  * A mail domain name for the mailing list service.
    See also "[Requirements](../requirements.md#network-requirements)".

    Through the instructions in this chapter, ``mail.example.org`` will be
    used for example.

Instruction by MTAs
-------------------

  - ~~[exim](configure-mail-server-exim.md)~~ (Work in progress)
  - ~~[OpenSMTPD](configure-mail-server-opensmtpd.md)~~ (Work in progress)
  - [Postfix](configure-mail-server-postfix.md)
  - [Sendmail](configure-mail-server-sendmail.md)

Tests
-----

  1. Start mail user agent (MUA) on your PC or PDA.

  2. Send any message to ``sympa-request@mail.example.org``.
     And confirm that your message will be delivered to ``postmaster``.

  3. Send a message with a subject "help" to ``sympa@mail.example.org``.
     And confirm that the message will be stored into
     [incoming spool directory](../man/sympa.conf.5.md#queue) (by default
     it is ``msg`` subdirectory in [``$SPOOLDIR``](../layout.md#spooldir)
     directory).

     ----
     Note:

       * Afterward when the service will start, this message will be processed
         by Sympa and a help messsage will be sent back to you.

     ----

  4. Send any message to ``bounce+hogehoge@mail.example.org``.
     And confirm that the message will be stored into
     [bounce spool directory](../man/sympa.conf.5.md#bouncequeue) (by default
     it is ``bounce`` subdirectory in [``$SPOOLDIR``](../layout.md#spooldir)
     directory).

     ----
     Note:

       * Afterward when the service will start, this message will be processed
         by Sympa and moved into ``bad`` subdirectory of the spool.

     ----

If something went unexpected, check mail system log and configuration of MTA.

