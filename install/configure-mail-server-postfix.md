Configure mail server: Postfix
==============================

Single domain (using ``postalias``)
-----------------------------------

1. Edit /etc/sympa/sympa.conf to add following line:
   ```
   aliases_program postalias
   ```

2. Copy [``/etc/sympa/aliases.sympa.postfix``](../examples/postfix/single-domain/aliases.sympa.postfix) file and edit it as you prefer.

   If sympa_aliases file does not exist, create it:
   ```
   # touch /var/lib/sympa/sympa_aliases
   # chmod 640 /var/lib/sympa/sympa_aliases
   # chown sympa:sympa /var/lib/sympa/sympa_aliases
   ```

3. Edit main.cf:

   * Add maps to alias_maps parameter:
     ```
     alias_maps = (...existing parameter value...),
       hash:/etc/sympa/aliases.sympa.postfix,
       hash:/var/lib/sympa/sympa_aliases
     ```

   * Add a map to alias database:
     ```
     alias_database = (...existing parameter value...),
       hash:/etc/sympa/aliases.sympa.postfix
     ```

   * Add following line to enable recipient delimiter:
     ```
     recipient_delimiter = +
     ```

4. Create alias databases:
   ```
   # newaliases
   # su sympa -c /usr/sbin/sympa_newaliases.pl
   ```

5. Reload Postfix.

---
N.B.: This instruction is to use postalias(1) maintenance tool.  If you
wish to use postmap(1), edit /etc/sympa/sympa.conf to add a line
``aliases_program postmap``.

