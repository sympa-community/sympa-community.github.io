---
title: 'Configure mail server: Postfix'
prev: configure-system-log.md
up: configure-mail-server.md
next: configure-http-server.md
---

Configure mail server: Postfix
==============================

Requirements
------------

* [Postfix](http://www.postfix.org/).

* A mail domain name for the mailing list service.
  See also "[Requirements](../requirements.md#network-requirements)".

  In the instructions below, ``mail.example.org`` will be used for example.

Two ways to integrate
---------------------

There are two ways to integrate Sympa into Postfix:
* _Fully virtual_ setting (using ``postmap`` and transports).
* _Single domain_ setting (using ``postalias`` and alias database).

The former is recommended.  However, if you will never have plan to manage
multiple domains, the latter is easier way.

Fully virtual setting
---------------------

### Initial setting

Steps in this section may be done once at the first time.

1. Copy an
   [example ``list_aliases.tt2``](../examples/postfix/virtual/list_aliases.tt2)
   file into [``$SYSCONFDIR``](../layout.md#sysconfdir) directory and edit it
   as you prefer.

   Edit [``sympa.conf``](../layout.md#config) to add following lines (Note:
   replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below):
   ```
   sendmail_aliases $SYSCONFDIR/sympa_transport
   aliases_program postmap
   aliases_db_type hash
   ```
   By these settings, ``sympa_transport`` file will be updated automatically
   when any lists are created, closed, restored or purged.

2. Create empty map files (Note:
   replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below):
   ```
   # touch $SYSCONFDIR/transport.sympa
   # touch $SYSCONFDIR/virtual.sympa
   # touch $SYSCONFDIR/sympa_transport
   # chmod 640 $SYSCONFDIR/sympa_transport
   # chown sympa:sympa $SYSCONFDIR/sympa_transport
   ```
   and create databases (Note:
   replace [``$SYSCONFDIR``](../layout.md#sysconfdir)):
   ```
   # postmap hash:$SYSCONFDIR/transport.sympa
   # postmap hash:$SYSCONFDIR/virtual.sympa
   # sympa_newaliases.pl
   ```

3. Edit master.cf to add transport definitions (Note:
   replace [``$LIBEXECDIR``](../layout.md#libexecdir) below):
   ```
   sympa   unix    -       n       n       -       -       pipe
     flags=hqRu user=sympa argv=$LIBEXECDIR/queue ${nexthop}
   sympabounce unix -      n       n       -       -       pipe
     flags=hqRu user=sympa argv=$LIBEXECDIR/bouncequeue ${nexthop}
   ```
   Note that ``flags`` option have to contain ``R``. ``F`` is unnecessary.

4. Edit Postfix main.cf file to add configuration for virtual domains (Note:
   replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below):
   ```
   # virtual(8) maps
   virtual_mailbox_domains = (...existing parameter value...),
     hash:$SYSCONFDIR/transport.sympa
   virtual_mailbox_maps = (...existing parameter value...),
     hash:$SYSCONFDIR/transport.sympa,
     hash:$SYSCONFDIR/sympa_transport,
     hash:$SYSCONFDIR/virtual.sympa
   # virtual(5) maps
   virtual_alias_maps = (...existing parameter value...),
     hash:$SYSCONFDIR/virtual.sympa
   # transport maps
   transport_maps = (...existing parameter value...),
     hash:$SYSCONFDIR/transport.sympa,
     hash:$SYSCONFDIR/sympa_transport
   # For VERP
   recipient_delimiter = +
   ```
   ----
   Note:
   * If
     [``mydestination``](http://www.postfix.org/postconf.5.html#mydestination)
     parameter in main.cf includes the virtual domain listed in
     ``virtual_mailbox_domains``, Postfix outputs warnings to system log.
     Remove virtual domain(s) from ``mydestination``.

   ----

### Adding new domain

Steps in this section have to be done every time the new domain is added.

1. Create directories for virtual domain configurations (Note:
   replace [``$SYSCONFDIR``](../layout.md#sysconfdir),
   [``$EXPLDIR``](../layout.md#expldir) and ``mail.example.org`` below):
   ```
   # mkdir -m 755 $SYSCONFDIR/mail.example.org
   # touch $SYSCONFDIR/mail.example.org/robot.conf
   # chown -r sympa:sympa $SYSCONFDIR/mail.example.org
   # mkdir -m 750 $EXPLDIR/mail.example.org
   # chown sympa:sympa $EXPLDIR/mail.example.org
   ```

2. Add following contents to ``transport.sympa`` and ``virtual.sympa``
   and edit them as you prefer (Note: replace ``mail.example.org`` below).

   ``transport.sympa``:
   ```
   mail.example.org                error:User unknown in recipient table
   sympa@mail.example.org          sympa:sympa@mail.example.org
   listmaster@mail.example.org     sympa:listmaster@mail.example.org
   bounce@mail.example.org         sympabounce:sympa@mail.example.org
   abuse-feedback-report@mail.example.org  sympabounce:sympa@mail.example.org

   ```

   ``virtual.sympa``:
   ```
   sympa-request@mail.example.org  postmaster@localhost
   sympa-owner@mail.example.org    postmaster@localhost

   ```

   Then, update databases for transport map and virtual alias map (Note:
   replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below):
   ```
   # postmap hash:$SYSCONFDIR/transport.sympa
   # postmap hash:$SYSCONFDIR/virtual.sympa
   ```

3. Reload Postfix.

Single domain setting
---------------------

1. Edit [``sympa.conf``](../layout.md#config) to add following lines (Note:
   replace ``mail.example.org``):
   ```
   domain mail.example.org
   aliases_program postalias
   ```

2. Save following excerpt as ``aliases.sympa.postfix`` file in
   [``$SYSCONFDIR``](../layout.md#sysconfdir) and edit it as you prefer (Note:
   replace [``$LIBEXECDIR``](../layout.md#libexecdir) and ``mail.example.org``
   below):
   ```
   # Robot aliases for Sympa.
   sympa:                 "| $LIBEXECDIR/queue sympa@mail.example.org"
   listmaster:            "| $LIBEXECDIR/queue listmaster@mail.example.org"
   bounce:                "| $LIBEXECDIR/bouncequeue sympa@mail.example.org"
   abuse-feedback-report: "| $LIBEXECDIR/bouncequeue sympa@mail.example.org"
   sympa-request:         postmaster
   sympa-owner:           postmaster
   #listserv:	          sympa
   #listserv-request:     sympa-request
   #majordomo:            sympa
   #listserv-owner:       sympa-owner
   ```

   If [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) file does not
   exist, create it (Note:
   replace [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) below):
   ```
   # touch $SENDMAIL_ALIASES
   # chmod 640 $SENDMAIL_ALIASES
   # chown sympa:sympa $SENDMAIL_ALIASES
   ```
   and create alias databases (Note:
   replace [``$SYSCONFDIR``](../layout.md#sysconfdir)):
   ```
   # postalias hash:$SYSCONFDIR/aliases.sympa.postfix
   # sympa_newaliases.pl
   ```

3. Edit Postfix main.cf file (Note:
   replace [``$SYSCONFDIR``](../layout.md#sysconfdir),
   [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) and
   ``mail.example.org`` below):
   ```
   mydestination = (...existing parameter value...), mail.example.org
   alias_maps = (...existing parameter value...),
     hash:$SYSCONFDIR/aliases.sympa.postfix,
     hash:$SENDMAIL_ALIASES
   alias_database = (...existing parameter value...),
     hash:$SYSCONFDIR/aliases.sympa.postfix
   recipient_delimiter = +
   ```

4. Reload Postfix.

