Configure mail server: Sendmail
===============================

  * Edit /etc/sympa/aliases.sympa.sendmail file as you prefer.

  * Edit /etc/mail/sendmail.cf:

    * Add following paths to AliasFile:

      * /etc/sympa/aliases.sympa.sendmail

      * /var/lib/sympa/sympa_aliases

    * Or, if you are generating sendmail.cf by cf package, edit sendmail.mc
      to add paths above to argument of ``ALIAS_FILE`` macro.

  * Run ``newaliases``.

  * Run ``service sendmail condrestart``.

N.B.: This instruction is to use newaliases(1) maintainance tool.  If you
wish to use makemap(1), edit /etc/sympa/sympa.conf to add a line
``aliases_program makemap``.

