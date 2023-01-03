---
title: 'Configure mail server: Sendmail'
prev: configure-system-log.md
up: configure-mail-server.md
next: configure-mail-server.md#tests
---

Configure mail server: Sendmail
===============================

Requirements
------------

  * [Sendmail](https://www.proofpoint.com/us/sendmail-open-source)
    is installed and confirmed to work.

  * A mail domain name for the mailing list service.
    See also "[Requirements](../requirements.md#network-requirements)".

    In the instructions below, ``mail.example.org`` will be used for example.

General instruction
-------------------

  1. Set [``domain``](/gpldoc/man/sympa_config.5.html#domain) parameter.
     Add following line to [``sympa.conf``](/gpldoc/man/sympa_config.5.html#config)
     (Note: replace ``mail.example.org``):
     ``` code
     domain mail.example.org
     ```
     Also, if path of ``sendmail`` executable file is differ from the default
     value of [``sendmail``](/gpldoc/man/sympa_config.5.html#sendmail) parameter,
     ``/usr/sbin/sendmail``, define it.  For example:
     ``` code
     sendmail /usr/lib/sendmail
     ```

  2. Edit sendmail.cf (Note:
     replace [``$SYSCONFDIR``](../layout.md#sysconfdir) and
     [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) below):

       * Add ``AliasFile`` lines to sendmail.cf:
         ``` code
         O AliasFile=$SYSCONFDIR/aliases.sympa.sendmail
         O AliasFile=$SENDMAIL_ALIASES
         ```

       * Or, if you are generating sendmail.cf by "cf" package, edit
         sendmail.mc to add paths to argument of ``ALIAS_FILE`` macro:
         ``` code
         define(`ALIAS_FILE', `(...exisitng value...),$SYSCONFDIR/aliases.sympa.sendmail,$SENDMAIL_ALIASES')
         ```
         then recompile sendmail.cf.

  3. Save following excerpt as ``aliases.sympa.sendmail`` file in
     [``$SYSCONFDIR``](../layout.md#sysconfdir) and edit it as you prefer
     (Note: replace [``$LIBEXECDIR``](../layout.md#libexecdir) and
     ``mail.example.org`` below):
     ``` code
     # Service aliases for Sympa.
     sympa:                 "| $LIBEXECDIR/queue sympa@mail.example.org"
     listmaster:            "| $LIBEXECDIR/queue listmaster@mail.example.org"
     bounce+*:              "| $LIBEXECDIR/bouncequeue sympa@mail.example.org"
     abuse-feedback-report: "| $LIBEXECDIR/bouncequeue sympa@mail.example.org"
     sympa-request:         postmaster
     sympa-owner:           postmaster
     #listserv:             sympa
     #listserv-request:     sympa-request
     #majordomo:            sympa
     #listserv-owner:       sympa-owner
     ```

  4. Create empty [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) file
     (Note: replace [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases)
     below):
     ``` bash
     touch $SENDMAIL_ALIASES
     chmod 640 $SENDMAIL_ALIASES
     chown sympa:sympa $SENDMAIL_ALIASES
     ```
     then create alias databases:
     ```
     # newaliases
     ```

  5. Restart Sendmail.

