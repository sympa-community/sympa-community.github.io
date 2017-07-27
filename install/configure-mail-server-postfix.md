Configure mail server: Postfix
==============================

Single domain (using ``postalias``)
-----------------------------------

  * Edit /etc/sympa/sympa.conf to add following line:
    ```
    aliases_program postalias
    ```

  * Edit /etc/sympa/aliases.sympa.postfix file as you prefer.

  * Edit /etc/postfix/main.cf:

    * Add following maps to $alias_maps:

      * hash:/etc/sympa/aliases.sympa.postfix

      * hash:/var/lib/sympa/sympa_aliases

    * Add following map to $alias_database:

      * hash:/etc/sympa/aliases.sympa.postfix

    * Add following line:
      ```
      recipient_delimiter = +
      ```

  * Run ``newaliases``.

  * Run ``su sympa -c /usr/sbin/sympa_newaliases.pl``.

  * Run ``service postfix condrestart``.

N.B.: This instruction is to use postalias(1) maintainance tool.  If you
wish to use postmap(1), edit /etc/sympa/sympa.conf to add a line
``aliases_program postmap``.

