Configure mail server: Sendmail
===============================

Requirements
------------

* [Sendmail](https://www.proofpoint.com/us/sendmail-open-source)
  is installed and confirmed to work.

* Mail domain for mailing list service.  Appropriate DNS resource record
  (``MX``, ``A`` or ``AAAA``) should be assigned and accessible via Internet.
  In the instructions below, ``mail.example.org`` is used.

General instruction
-------------------

1. Set [``domain``](../man/sympa.conf.5.md#domain) parameter.
   Add following line to sympa.conf:
   ```
   domain mail.example.org
   ```

2. Edit sendmail.cf (Note:
   replace [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) below.
   And note that ``/etc/mail`` is Sendmail configuation directory):

   * Add ``AliasFile`` lines to sendmail.cf:
     ```
     O AliasFile=/etc/mail/aliases.sympa.sendmail
     O AliasFile=$SENDMAIL_ALIASES
     ```

   * Or, if you are generating sendmail.cf by "cf" package, edit sendmail.mc
     to add paths to argument of ``ALIAS_FILE`` macro:
     ```
     define(`ALIAS_FILE', `(...exisitng value...),/etc/mail/aliases.sympa.sendmail,$SENDMAIL_ALIASES')
     ```
     then recompile sendmail.cf.

3. Save following excerpt as ``aliases.sympa.sendmail`` file in Sendmail
   configuration directory (such as ``/etc/mail``) and edit it as you prefer
   (Note: replace [``$LIBEXECDIR``](../layout.md#libexecdir) below):
   ```
   # Robot aliases for Sympa.
   sympa:                 "| $LIBEXECDIR/queue sympa@mail.example.org"
   listmaster:            "| $LIBEXECDIR/queue listmaster@mail.example.org"
   bounce+*:              "| $LIBEXECDIR/bouncequeue sympa@mail.example.org"
   abuse-feedback-report: "| $LIBEXECDIR/bouncequeue sympa@mail.example.org"
   sympa-request:         postmaster
   sympa-owner:           postmaster
   #listserv:	          sympa
   #listserv-request:     sympa-request
   #majordomo:            sympa
   #listserv-owner:       sympa-owner
   ```

4. Create empty [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) file
   (Note: replace [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases)
   below):
   ```
   touch $SENDMAIL_ALIASES
   chmod 640 $SENDMAIL_ALIASES
   chown sympa:sympa $SENDMAIL_ALIASES
   ```
   then create alias databases:
   ```
   # newaliases
   ```

5. Restart Sendmail.

