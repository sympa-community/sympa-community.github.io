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

    In the instructions below, ``mail.example.org`` will be used for example.$

  * [``$SYSCONFDIR``](../layout.md#sysconfdir) is the samba configuration
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

  2. Add the following to `sympa` configuration file [``sympa.conf``](../layout.md#config)

     ``` code
     sendmail_aliases $SENDMAIL_ALIASES.db
     aliases_program newaliases
     aliases_db_type hash
     ```

  3. It is seem that the alias file is created with .db extension. Do a link to
     the original [``$SENDMAIL_ALIASES``](../layout.md#sendmail_aliases) to
     ensure aliases is understood by both program

     ``` code
     ln -s "$SENDMAIL_ALIASES" "$SENDMAIL_ALIASES".db
     ```

  4. Create the `list_aliases.tt2` template file in [``$SYSCONFDIR``](../layout.md#sysconfdir)
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

`Exim4` configuration steps

  1. `Exim4` may work with a single monolithic configuration template file or
     with several spited files according to your installation choice. If you
     choose single monolithic way, you have to edit the
     ``/etc/exim4/exim4.conf.template`` and put the following configuration
     after the `system_aliases` section.

     However, if you chose to split exim4
     configuration template in several file, you have to create a new file in
     router directory, named for instance:
     ``/etc/exim4/conf.d/router/401_exim4-config_sympa_aliases``.
     In doubt, you can make the both way.

  2. So add the following section to the `exim4` configuration

     ``` code
     sympa_aliases:
       debug_print = "R: sympa_aliases for $local_part@$domain"
       driver = redirect
       allow_fail
       allow_defer
       data = ${lookup{$local_part@$domain}lsearch{$SENDMAIL_ALIASES.db}}
       user = sympa
       group = sympa
       file_transport = address_file
       pipe_transport = address_pipe
     ```

  3. Run update-exim4 program to regenerate configuration

     ``` code
     update-exim4.conf
     ```

  4. Add `sympa` in system aliases, in /etc/aliases

     ``` code
     sympa: "| $LIBEXECDIR/queue sympa"
     sympa-request: "| $LIBEXECDIR/queue sympa"
     sympa-owner: "| $LIBEXECDIR/queue sympa"
     sympa-listmaster: postmaster

     postmaster: postmaster@mail.example.org
     ```

  5. Restart `sympa` and `exim4` service

     ``` code
     /etc/init.d/sympa restart
     /etc/init.d/exim4 restart
     ```

  6. Test the routing plan

     ``` code
     exim4 -bt mail.example.org
     ```
