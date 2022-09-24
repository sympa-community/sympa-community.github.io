---
title: 'Configure mail server: Exim4'
prev: configure-system-log.md
up: configure-mail-server.md
next: configure-mail-server.md#tests
---

Configure mail server: Exim4
============================

Requirements
------------

  * [Exim4](https://www.exim.org/).

  * A mail domain name for the mailing list service.
    See also "[Requirements](../requirements.md#network-requirements)".

    In the instructions below, ``mail.example.org`` will be used for example.

  * [``$SYSCONFDIR``](../layout.md#sysconfdir) is the Sympa configuration
    directory.
    [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) designated
    file alias.
    [``$LIBEXECDIR``](../layout.md#libexecdir) is the execution directory.
    You have to replace these symboles by their real value.

Virtual domain setting
----------------------
### Initial setting

Steps in this section may be done once at the first time.

  1. If path of ``sendmail`` executable file is differ from the default value
     of [``sendmail``](/gpldoc/man/sympa_config.5.html#sendmail) parameter,
     ``/usr/sbin/sendmail``, define it in
     [``sympa.conf``](../layout.md#config).  For example:

     ``` code
     sendmail /usr/local/sbin/sendmail
     ```

  2. Add the following to `Sympa` configuration file [``sympa.conf``](../layout.md#config)

     ``` code
     sendmail_aliases $SENDMAIL_ALIASES
     aliases_program none
     ```

  3. Create the `list_aliases.tt2` template file in [``$SYSCONFDIR``](../layout.md#sysconfdir)
     directory with following content:

     ``` code
     #--- [% list.name %]@[% list.domain %]: list map created at [% date %]
     [% list.name %]@[% list.domain %]: "| $LIBEXECDIR/queue [% list.name %]@[% list.domain %]"
     [% list.name %]-request@[% list.domain %]: "| $LIBEXECDIR/queue [% list.name %]-request@[% list.domain %]"
     [% list.name %]-editor@[% list.domain %]: "| $LIBEXECDIR/queue [% list.name %]-editor@[% list.domain %]"
     [% list.name %]-subscribe@[% list.domain %]: "|  $LIBEXECDIR/queue [% list.name %]-subscribe@[%list.domain %]"
     [% list.name %]-unsubscribe@[% list.domain %]: "|  $LIBEXECDIR/queue [% list.name %]-unsubscribe@[% list.domain %]"
     [% list.name %][% return_path_suffix %]@[% list.domain %]: "|  $LIBEXECDIR/bouncequeue [% list.name %]@[% list.domain %]"
     ```

Exim4 configuration
-------------------
### Exim4 Architecture configuration

  _Exim4_ may work with a single monolithic configuration template file or
  with several splitted files according to your installation choice.
  Filling the bad the configuration have no effect.

  If you choose single monolithic way, you have to edit the
  ``/etc/exim4/exim4.conf.template`` and put the router configuration part after
  "``begin router``" message and before "``begin ``_otherThing_ ". You can put it
  typically next to the "``system_aliases``" section. The transport configuration
  should take place after "``begin transport``", typically next to the
  "``address_directory``" section.

  However, if you choose to split _Exim4_ configuration template in several file,
  you have to create a new router file under `router` directory, for instance
  ``/etc/exim4/conf.d/router/401_exim4-config_sympa_aliases``, and a new
  transport file under `transport` directory like
  ``/etc/exim4/conf.d/transport/401_exim4-config_sympa_transport``

### Router part

  Is it unnecessary to filling /etc/aliases with _Sympa_ address like
  ``sympa: "| $LIBEXECDIR/queue sympa`` as theses aliases will be managed into
  exim configuration file.

  1. **Routing the _Sympa_ common aliases**
     This describe the _Sympa_ routing aliases. Transport part will be defined after.

     ``` code
     # in "begin router"

     # Sympa generic aliases for sympa, sympa-request, sympa-owner, listmaster
     sympa_queue_aliases:
       debug_print = "R: sympa queue aliases for $local_part@$domain"
       driver = accept
       domains = mail.example.org
       local_parts = sympa : sympa-request : sympa-owner : sympa-owner : listmaster
       transport = sympa_queue_transport
       no_more

     # Sympa generic aliases for "bounce" and "abuse-feedback-report"
     sympa_bouncequeue_aliases:
       debug_print = "R: sympa bouncequeue aliases for $local_part@$domain"
       driver = accept
       domains = mail.example.org
       local_parts = bounce : abuse-feedback-report
       transport = sympa_bounce_queue_transport
       no_more
     ```

  2. **Routing mailing list**
     Tell to _Exim4_ to use the _Sympa_ mailing list file.

     ``` code
     # Sympa list aliases
     sympa_aliases:
       debug_print = "R: sympa_aliases for $local_part@$domain"
       driver = redirect
       allow_fail
       allow_defer
       require_files = "+$SENDMAIL_ALIASES"
       data = ${lookup{$local_part@$domain}lsearch{$SENDMAIL_ALIASES}}
       user = sympa
       group = sympa
       file_transport = address_file
       pipe_transport = address_pipe
       no_more
     ```

### Transport part

  Tell to _Exim4_ the definition of _Sympa_ command. Should be added in transport
  section.

  ``` code
  # begin transports

  # Sympa transport for queue program
  sympa_queue_transport:
    driver = pipe
    command = /usr/lib/sympa/bin/queue ${local_part}\@$domain
    user = sympa
    group = sympa
    return_fail_output
    return_path_add

  # Sympa transport for bouncequeue program
  sympa_bounce_queue_transport:
    driver = pipe
    command = /usr/lib/sympa/bin/bouncequeue ${local_part}\@$domain
    user = sympa
    group = sympa
    return_fail_output
    return_path_add
  ```

### System steps

  1. Run _update-exim4_ program to regenerate the _Exim4_ configuration

     ``` code
     update-exim4.conf
     ```

  2. Restart `sympa` and `exim4` service

     ``` code
     /etc/init.d/sympa restart
     /etc/init.d/exim4 restart
     ```

  3. Test the routing plan

     ``` code
     exim4 -bt list@mail.example.org
     ```
