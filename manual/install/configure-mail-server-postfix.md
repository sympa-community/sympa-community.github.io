---
title: 'Configure mail server: Postfix'
prev: configure-system-log.md
up: configure-mail-server.md
next: configure-mail-server.md#tests
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

  * [_Virtual domain_ setting](#virtual-domain-setting) (using ``postmap`` and transports).
  * [_Single domain_ setting](#single-domain-setting) (using ``postalias`` and alias database).

The former is recommended.  However, if you will never have plan to manage
multiple domains, the latter is easier way.

You can not mix both ways.  Following sections describe these two ways by each.

Virtual domain setting
----------------------

### Initial setting

Steps in this section may be done once at the first time.

  1. If path of ``sendmail`` executable file is differ from the default value
     of [``sendmail``](../man/sympa.conf.5.md#sendmail) parameter,
     ``/usr/sbin/sendmail``, define it in
     [``sympa.conf``](../layout.md#config).  For example:

     ``` code
     sendmail /usr/local/sbin/sendmail
     ```

  2. Create `list_aliases.tt2` file in
     [``$SYSCONFDIR``](../layout.md#sysconfdir) directory with following
     content:
     ``` code
     #--- [% list.name %]@[% list.domain %]: list transport map created at [% date %]
     [% list.name %]@[% list.domain %] sympa:[% list.name %]@[% list.domain %]
     [% list.name %]-request@[% list.domain %] sympa:[% list.name %]-request@[% list.domain %]
     [% list.name %]-editor@[% list.domain %] sympa:[% list.name %]-editor@[% list.domain %]
     #[% list.name %]-subscribe@[% list.domain %] sympa:[% list.name %]-subscribe@[%list.domain %]
     [% list.name %]-unsubscribe@[% list.domain %] sympa:[% list.name %]-unsubscribe@[% list.domain %]
     [% list.name %][% return_path_suffix %]@[% list.domain %] sympabounce:[% list.name %]@[% list.domain %]
     ```
     and edit it as you prefer.

     Edit [``sympa.conf``](../layout.md#config) to add following lines (Note:
     replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below):
     ``` code
     sendmail_aliases $SYSCONFDIR/sympa_transport
     aliases_program postmap
     aliases_db_type hash
     ```
     By these settings, ``sympa_transport`` file will be updated automatically
     when any lists are created, closed, restored or purged.

  3. Create empty map files (Note:
     replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below):
     ``` bash
     # touch $SYSCONFDIR/transport.sympa
     # touch $SYSCONFDIR/virtual.sympa
     # touch $SYSCONFDIR/sympa_transport
     # chmod 640 $SYSCONFDIR/sympa_transport
     # chown sympa:sympa $SYSCONFDIR/sympa_transport
     ```
     and create databases (Note:
     replace [``$SYSCONFDIR``](../layout.md#sysconfdir)):
     ``` bash
     # postmap hash:$SYSCONFDIR/transport.sympa
     # postmap hash:$SYSCONFDIR/virtual.sympa
     # sympa_newaliases.pl
     ```

  4. Edit Postfix ``master.cf`` file to add transport definitions (Note:
     replace [``$LIBEXECDIR``](../layout.md#libexecdir) below):
     ``` code
     sympa   unix    -       n       n       -       -       pipe
       flags=hqRu user=sympa argv=$LIBEXECDIR/queue ${nexthop}
     sympabounce unix -      n       n       -       -       pipe
       flags=hqRu user=sympa argv=$LIBEXECDIR/bouncequeue ${nexthop}
     ```
     Note that ``flags`` option have to contain ``R``. ``F`` is unnecessary.

  5. Edit Postfix ``main.cf`` file to add configuration for virtual domains
     (Note: replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below):
     ``` code
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
         parameter in ``main.cf`` file includes the virtual domain listed in
         ``virtual_mailbox_domains``, Postfix outputs warnings to system log.
         Remove virtual domain(s) from ``mydestination``.

     ----

### Adding new domain

Steps in this section have to be done every time the new domain is added.

  1. Create directories for virtual domain configurations (Note:
     replace [``$SYSCONFDIR``](../layout.md#sysconfdir),
     [``$EXPLDIR``](../layout.md#expldir) and ``mail.example.org`` below):
     ``` bash
     # mkdir -m 755 $SYSCONFDIR/mail.example.org
     # touch $SYSCONFDIR/mail.example.org/robot.conf
     # chown -r sympa:sympa $SYSCONFDIR/mail.example.org
     # mkdir -m 750 $EXPLDIR/mail.example.org
     # chown sympa:sympa $EXPLDIR/mail.example.org
     ```

  2. If you want to override global settings in
     [``sympa.conf``](../layout.md#config) (such as
     [``lang``](../man/sympa.conf.5.md#lang)) by each domain, you can add it
     to ``robot.conf`` file above.

  3. If Sympa services have already been running, reload them
     (see "[Reloading Sympa services](../admin/services.md#reloading-sympa-services)").

  4. Add following contents to ``transport.sympa`` and ``virtual.sympa`` files
     and edit them as you prefer (Note: replace ``mail.example.org`` below).

     ``transport.sympa``:
     ``` code
     mail.example.org                error:User unknown in recipient table
     sympa@mail.example.org          sympa:sympa@mail.example.org
     listmaster@mail.example.org     sympa:listmaster@mail.example.org
     bounce@mail.example.org         sympabounce:sympa@mail.example.org
     abuse-feedback-report@mail.example.org  sympabounce:sympa@mail.example.org

     ```

     ``virtual.sympa``:
     ``` code
     sympa-request@mail.example.org  postmaster@localhost
     sympa-owner@mail.example.org    postmaster@localhost

     ```

     ----
     Note:

       * If you want some addresses of ``mail.example.org`` to be excluded
         from mailing list service, you can add them to these files.

     ----

     Then, update databases for transport map and virtual alias map (Note:
     replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below):
     ``` bash
     # postmap hash:$SYSCONFDIR/transport.sympa
     # postmap hash:$SYSCONFDIR/virtual.sympa
     ```

  5. Reload Postfix.
     Then test configuration according to
     [instruction](configure-mail-server.md#tests).

Single domain setting
---------------------

  1. Edit [``sympa.conf``](../layout.md#config) to add following lines (Note:
     replace ``mail.example.org``):
     ``` code
     domain mail.example.org
     aliases_program postalias
     sendmail /usr/local/sbin/sendmail  (If path is differ from the default)
     ```

  2. Save following excerpt as ``aliases.sympa.postfix`` file in
     [``$SYSCONFDIR``](../layout.md#sysconfdir) and edit it as you prefer
     (Note: replace [``$LIBEXECDIR``](../layout.md#libexecdir) and
     ``mail.example.org`` below):
     ``` code
     # Robot aliases for Sympa.
     sympa:                 "| $LIBEXECDIR/queue sympa@mail.example.org"
     listmaster:            "| $LIBEXECDIR/queue listmaster@mail.example.org"
     bounce:                "| $LIBEXECDIR/bouncequeue sympa@mail.example.org"
     abuse-feedback-report: "| $LIBEXECDIR/bouncequeue sympa@mail.example.org"
     sympa-request:         postmaster
     sympa-owner:           postmaster
     #listserv:             sympa
     #listserv-request:     sympa-request
     #majordomo:            sympa
     #listserv-owner:       sympa-owner
     ```

     If [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) file does not
     exist, create it (Note:
     replace [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) below):
     ``` bash
     # touch $SENDMAIL_ALIASES
     # chmod 640 $SENDMAIL_ALIASES
     # chown sympa:sympa $SENDMAIL_ALIASES
     ```
     and create alias databases (Note:
     replace [``$SYSCONFDIR``](../layout.md#sysconfdir)):
     ``` bash
     # postalias hash:$SYSCONFDIR/aliases.sympa.postfix
     # sympa_newaliases.pl
     ```

  3. Edit Postfix main.cf file (Note:
     replace [``$SYSCONFDIR``](../layout.md#sysconfdir),
     [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) and
     ``mail.example.org`` below):
     ``` code
     mydestination = (...existing parameter value...), mail.example.org
     alias_maps = (...existing parameter value...),
       hash:$SYSCONFDIR/aliases.sympa.postfix,
       hash:$SENDMAIL_ALIASES
     alias_database = (...existing parameter value...),
       hash:$SYSCONFDIR/aliases.sympa.postfix
     recipient_delimiter = +
     ```

  4. Reload Postfix.
     Then test configuration according to
     [instruction](configure-mail-server.md#tests).

