---
title: 'Configure mail server: OpenSMTPD'
prev: configure-system-log.md
up: configure-mail-server.md
next: configure-mail-server.md#tests
---

Configure mail server: OpenSMTPD
================================

Requirements
------------

  * [OpenSMTPD](http://www.opensmtpd.org/).

  * A mail domain name for the mailing list service.
    See also "[Requirements](../requirements.md#network-requirements)".

    In the instructions below, ``mail.example.org`` will be used for example.

  * If you are planning to use
    [message tracking](../customize/message-tracking.md)
    feature, [smtpc](https://github.com/ikedas/smtpc).
    See the [description](../customize/lmtp-delivery.md#installation) to
    get and install it.

Sympa configuration parameters
------------------------------

  * [``aliases_program``](/gpldoc/man/sympa_config.5.html#aliases_program)

    This parameter must be set to full path to makemap(1) utility bundled in
    OpenSMTPD:
    ``` code
    aliases_program /path/to/makemap
    ```
    Note that setting a keyword ``makemap`` _will not_ work.

  * [``sendmail``](/gpldoc/man/sympa_config.5.html#sendmail) and
    [``sendmail_args``](/gpldoc/man/sympa_config.5.html#sendmail_args)

      - If you are planning to use
        [message tracking](../customize/message-tracking.md)
        feature, add these lines:
        ``` code
        sendmail /path/to/sympa_smtpc
        sendmail_args --esmtp localhost
        ```
        Note that smtpd daemon must listen on ``localhost``.

      - Otherwise, add this line:
        ``` code
        sendmail /path/to/sendmail
        ```
        If the full path to sendmail(1) utility bundled in OpenSMTPD is the
        same as default of Sympa, ``/usr/sbin/sendmail``, the line above may
        not be added.

Setup
-----

### Initial setting

Steps in this section may be done once at the first time.

  1. Edit [``sympa.conf``](../layout.md#config) to add parameters described
     in the section above, for example:
     ``` code
     aliases_program /path/to/makemap
     sendmail /path/to/sendmail
     ```
     Then create ``list_aliases.tt2`` file in
     [``$SYSCONFDIR``](../layout.md#sysconfdir) directory with following
     content, and edit it as you prefer (Note:
     replace [``$LIBEXECDIR``](../layout.md#libexecdir) below):
     ``` code
     #--- [% list.name %]@[% list.domain %]: list virtual table created at [% date %]
     [% list.name %]@[% list.domain %]               "| $LIBEXECDIR/queue [% list.name %]@[% list.domain %]"
     [% list.name %]-request@[% list.domain %]       "| $LIBEXECDIR/queue [% list.name %]-request@[% list.domain %]"
     [% list.name %]-editor@[% list.domain %]        "| $LIBEXECDIR/queue [% list.name %]-editor@[% list.domain %]"
     #[% list.name %]-subscribe@[% list.domain %]    "| $LIBEXECDIR/queue [% list.name %]-subscribe@[%list.domain %]"
     [% list.name %]-unsubscribe@[% list.domain %]   "| $LIBEXECDIR/queue [% list.name %]-unsubscribe@[% list.domain %]"
     [% list.name %][% return_path_suffix %]@[% list.domain %] "| $LIBEXECDIR/bouncequeue [% list.name %]@[% list.domain %]"
     ```
     By these settings, [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases)
     file will be updated automatically when any lists are created, closed,
     restored or purged.

  2. Create empty virtual tables (Note:
     replace [``$SYSCONFDIR``](../layout.md#sysconfdir) and
     [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) below):
     ``` bash
     # touch $SYSCONFDIR/sympa_domain_aliases
     # chmod 644 $SYSCONFDIR/sympa_domain_aliases
     # touch $SENDMAIL_ALIASES
     # chmod 640 $SENDMAIL_ALIASES
     # chown sympa:smtpd $SENDMAIL_ALIASES
     ```
     and create databases (Note:
     replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below):
     ``` bash
     # makemap $SYSCONFDIR/sympa_domain_aliases
     # sympa_newaliases.pl
     ```
     Note that table files must be readable by smtpd (at least by ``smtpd``
     group).

  3. Edit OpenSMTPD ``smtpd.conf`` file to add configuration for virtual
     domains (Note: replace [``$SYSCONFDIR``](../layout.md#sysconfdir) and
     [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) below):
     ``` code
     table sympa db:$SYSCONFIDR/sympa_domain_aliases.db
     accept from any for any recipient <sympa> virtual <sympa>
     
     table lists db:$SENDMAIL_ALIASES.db
     accept from any for any recipient <lists> virtual <lists>
     
     #
     # Here may be the rules for addresses excluded from mailing list service.
     #
     
     table sympa_domains { "mail.example.org" }
     reject from any for domain <sympa_domains>
     ```
     Note: In the configuration above, brackets `<` ... `>` and their contents
     are not examples: They have to be typed as shown.

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
     [``lang``](/gpldoc/man/sympa_config.5.html#lang)) by each domain, you can add it
     to ``robot.conf`` above.

  3. If Sympa services have already been running, reload them (see
     "[Reloading Sympa services](../admin/services.md#reloading-sympa-services)").

  4. Edit ``smtpd.conf`` file to add the new domain to ``sympa_domains`` table:
     ``` code
     ...
     table sympa_domains { ...existing domains..., "mail.example.org" }
     ...
     ```

  5. Add following content to ``sympa_domain_aliases`` file
     and edit them as you prefer (Note:
     replace [``$LIBEXECDIR``](../layout.md#libexecdir) and
     ``mail.example.org`` below):
     ``` code
     sympa@mail.example.org             "| $LIBEXECDIR/queue sympa@mail.example.org"
     listmaster@mail.example.org        "| $LIBEXECDIR/queue listmaster@mail.example.org"
     bounce@mail.example.org            "| $LIBEXECDIR/bouncequeue sympa@mail.example.org"
     abuse-feedback-report@mail.example.org "| $LIBEXECDIR/bouncequeue sympa@mail.example.org"
     sympa-request@mail.example.org     postmaster@
     sympa-owner@mail.example.org       postmaster@
     #listserv@mail.example.org         sympa@mail.example.org
     #listserv-request@mail.example.org sympa-request@mail.example.org
     #majordomo@mail.example.org        sympa@mail.example.org
     #listserv-owner@mail.example.org   sympa-owner@mail.example.org

     ```
     Then, update database for virtual table (Note:
     replace [``$SYSCONFDIR``](../layout.md#sysconfdir) below):
     ``` bash
     # makemap $SYSCONFDIR/sympa_domain_aliases
     ```

  6. Reload OpenSMTPD.
     Then test configuration according to
     [instruction](configure-mail-server.md#tests).

