Configure mail server: Sendmail
===============================

General instruction
-------------------

1. Copy [example ``aliases.sympa.sendmail``](../examples/sendmail/aliases.sympa.sendmail``) file into /etc/sympa directory and edit it as you prefer.

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

3. Create alias databases:
   ```
   # newaliases
   ```

4. Restart Sendmail.

---
N.B.: This instruction is to use newaliases(1) maintenance tool.  If you
wish to use makemap(1), edit /etc/sympa/sympa.conf to add a line
``aliases_program makemap``.

