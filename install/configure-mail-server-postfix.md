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
   file into /etc/sympa directory and edit it as you prefer.

   Edit /etc/sympa/sympa.conf to add following lines:
   ```
   sendmail_aliases /var/lib/sympa/sympa_transport
   aliases_program postmap
   aliases_db_type hash
   ```
   By these settings, sympa_transport will be updated automatically when any
   lists are created, closed, restored or purged.

2. Create empty map files:
   ```
   # touch /etc/postfix/transport.sympa
   # touch /etc/postfix/virtual.sympa
   # touch /var/lib/sympa/sympa_transport
   # chmod 640 /var/lib/sympa/sympa_transport
   # chown sympa:sympa /var/lib/sympa/sympa_transport
   ```
   and create databases:
   ```
   # postmap hash:/etc/postfix/transport.sympa
   # postmap hash:/etc/postfix/virtual.sympa
   # sympa_newaliases.pl
   ```

3. Edit master.cf to add transport definitions:
   ```
   sympa   unix    -       n       n       -       -       pipe
     flags=hqRu user=sympa argv=/usr/libexec/sympa/queue ${nexthop}
   sympabounce unix -      n       n       -       -       pipe
     flags=hqRu user=sympa argv=/usr/libexec/sympa/bouncequeue ${nexthop}
   ```
   Note that ``flags`` option have to contain ``R``. ``F`` is unnecessary.

4. Edit main.cf to add configuration for virtual domains.
   ```
   # virtual(8) maps
   virtual_mailbox_domains = (...existing parameter value...),
     hash:/etc/postfix/transport.sympa
   virtual_mailbox_maps = (...existing parameter value...),
     hash:/etc/postfix/transport.sympa,
     hash:/var/lib/sympa/sympa_transport,
     hash:/etc/postfix/virtual.sympa
   # virtual(5) maps
   virtual_alias_maps = (...existing parameter value...),
     hash:/etc/postfix/virtual.sympa
   # transport maps
   transport_maps = (...existing parameter value...),
     hash:/etc/postfix/transport.sympa,
     hash:/var/lib/sympa/sympa_transport
   # For VERP
   recipient_delimiter = +
   ```
   ----
   Note:
   * If [``mydestination``](http://www.postfix.org/postconf.5.html#mydestination)
     parameter in main.cf includes the virtual domain listed in
     ``virtual_mailbox_domains``, Postfix outputs warnings to system log.
     Remove virtual domain(s) from ``mydestination``.
   ----

### Adding new domain

Steps in this section have to be done every time the new domain is added.

1. Create directories for virtual domain configurations:
   ```
   # mkdir -m 755 /etc/sympa/mail.example.org
   # touch /etc/sympa/mail.example.org/robot.conf
   # chown -r sympa:sympa /etc/sympa/mail.example.org
   # mkdir -m 750 /var/lib/sympa/list_data/mail.example.org
   # chown sympa:sympa /var/lib/sympa/list_data/mail.example.org
   ```

2. Add contents of
   [example ``transport.sympa``](../examples/postfix/virtual/transport.sympa)
   and [example ``virtual.sympa``](../examples/postfix/virtual/virtual.sympa)
   to the files in /etc/sympa directory and edit them as you prefer.

   Then, update databases for transport map and virtual alias map:
   ```
   # postmap hash:/etc/postfix/transport.sympa
   # postmap hash:/etc/postfix/virtual.sympa
   ```

3. Reload Postfix.

Single domain setting
---------------------

1. Edit /etc/sympa/sympa.conf to add following lines:
   ```
   domain mail.example.org
   aliases_program postalias
   ```

2. Copy [example ``aliases.sympa.postfix``](../examples/postfix/single/aliases.sympa.postfix) file into /etc/sympa directory and edit it as you prefer.
   If sympa_aliases file does not exist, create it:
   ```
   # touch /var/lib/sympa/sympa_aliases
   # chmod 640 /var/lib/sympa/sympa_aliases
   # chown sympa:sympa /var/lib/sympa/sympa_aliases
   ```
   and create alias databases:
   ```
   # newaliases
   # su sympa -c /usr/sbin/sympa_newaliases.pl
   ```

3. Edit main.cf:
   ```
   mydestination = (...existing parameter value...), mail.example.org
   alias_maps = (...existing parameter value...),
     hash:/etc/sympa/aliases.sympa.postfix,
     hash:/var/lib/sympa/sympa_aliases
   alias_database = (...existing parameter value...),
     hash:/etc/sympa/aliases.sympa.postfix
   recipient_delimiter = +
   ```

4. Reload Postfix.

