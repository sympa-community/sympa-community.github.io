Configure mail server: Postfix
==============================

Requirements
------------

* [Postfix](http://www.postfix.org/).

* Mail domain for mailing list service.  Appropriate DNS resource record
  (``MX``, ``A`` or ``AAAA``) should be assigned and accessible via Internet.
  In the instructions below, ``mail.example.org`` is used.

Two ways to integrate
---------------------

There are two ways to integrate Sympa to Postfix:
* _fully virtual_ setting (using ``postmap`` and transports).
* _single domain_ setting (using ``postalias`` and alias database).

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

   Edit [sympa.conf](../layout.md#config) to add following lines (Note:
   replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below):
   ```
   sendmail_aliases $SYSCONFDIR/sympa_transport
   aliases_program postmap
   aliases_db_type hash
   ```
   By these settings, sympa_transport will be updated automatically when any
   lists are created, closed, restored or purged.

2. Create empty map files (Note:
   replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below.  And note that
   ``/etc/postfix`` is config_directory of Postfix):
   ```
   # touch /etc/postfix/transport.sympa
   # touch /etc/postfix/virtual.sympa
   # touch $SYSCONFDIR/sympa_transport
   # chmod 640 $SYSCONFDIR/sympa_transport
   # chown sympa:sympa $SYSCONFDIR/sympa_transport
   ```
   and create databases:
   ```
   # postmap hash:/etc/postfix/transport.sympa
   # postmap hash:/etc/postfix/virtual.sympa
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
   replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below.  And note that
   ``/etc/postfix`` is config_directory of Postfix).
   ```
   # virtual(8) maps
   virtual_mailbox_domains = (...existing parameter value...),
     hash:/etc/postfix/transport.sympa
   virtual_mailbox_maps = (...existing parameter value...),
     hash:/etc/postfix/transport.sympa,
     hash:$SYSCONFDIR/sympa_transport,
     hash:/etc/postfix/virtual.sympa
   # virtual(5) maps
   virtual_alias_maps = (...existing parameter value...),
     hash:/etc/postfix/virtual.sympa
   # transport maps
   transport_maps = (...existing parameter value...),
     hash:/etc/postfix/transport.sympa,
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
   replace [``$SYSCONFDIR``](../layout.md#sysconfdir) and
   [``$EXPLDIR``](../layout.md#expldir)):
   ```
   # mkdir -m 755 $SYSCONFDIR/mail.example.org
   # touch $SYSCONFDIR/mail.example.org/robot.conf
   # chown -r sympa:sympa $SYSCONFDIR/mail.example.org
   # mkdir -m 750 $EXPLDIR/mail.example.org
   # chown sympa:sympa $EXPLDIR/mail.example.org
   ```

2. Copy
   [example ``transport.sympa``](../examples/postfix/virtual/transport.sympa)
   and [example ``virtual.sympa``](../examples/postfix/virtual/virtual.sympa)
   into config_directory of Postfix (such as ``/etc/postfix``) and edit them
   as you prefer.

   Then, update databases for transport map and virtual alias map:
   ```
   # postmap hash:/etc/postfix/transport.sympa
   # postmap hash:/etc/postfix/virtual.sympa
   ```

3. Reload Postfix.

Single domain setting
---------------------

1. Edit [sympa.conf](../layout.md#config) to add following lines:
   ```
   domain mail.example.org
   aliases_program postalias
   ```

2. Save following excerpt as ``aliases.sympa.postfix`` file in
   config_directory of Postfix (such as ``/etc/postfix``) and edit it as you
   prefer (Note: replace [``$LIBEXECDIR``](../layout.md#libexecdir)):
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
   and create alias databases:
   ```
   # newaliases
   # sympa_newaliases.pl
   ```

3. Edit Postfix main.cf file (Note:
   replace [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) below. And
   ``/etc/postfix`` is config_directory of Postfix):
   ```
   mydestination = (...existing parameter value...), mail.example.org
   alias_maps = (...existing parameter value...),
     hash:/etc/postfix/aliases.sympa.postfix,
     hash:$SENDMAIL_ALIASES
   alias_database = (...existing parameter value...),
     hash:/etc/postfix/aliases.sympa.postfix
   recipient_delimiter = +
   ```

4. Reload Postfix.

