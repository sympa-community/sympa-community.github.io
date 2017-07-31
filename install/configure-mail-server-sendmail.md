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

2. Edit sendmail.cf:

   * Add ``AliasFile`` lines to sendmail.cf:
     ```
     O AliasFile=/etc/sympa/aliases.sympa.sendmail
     O AliasFile=/var/lib/sympa/sympa_aliases
     ```

   * Or, if you are generating sendmail.cf by "cf" package, edit sendmail.mc
     to add paths to argument of ``ALIAS_FILE`` macro:
     ```
     define(`ALIAS_FILE', `(...exisitng value...),/etc/sympa/aliases.sympa.sendmail,/var/lib/sympa/sympa_aliases')
     ```
     then recompile sendmail.cf.

3. Copy [example ``aliases.sympa.sendmail``](../examples/sendmail/aliases.sympa.sendmail``) file into /etc/sympa directory and edit it as you prefer.

4. Create empty sympa_aliases file:
   ```
   touch /var/lib/sympa/sympa_aliases
   chmod 640 /var/lib/sympa/sympa_aliases
   chown sympa:sympa /var/lib/sympa/sympa_aliases
   ```
   then create alias databases:
   ```
   # newaliases
   ```

5. Restart Sendmail.

----
N.B.: This instruction is to use newaliases(1) maintenance tool.  If you
wish to use makemap(1), edit /etc/sympa/sympa.conf to add a line
``aliases_program makemap``.

----

