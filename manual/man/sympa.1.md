---
title: 'sympa(1)'
---

# NAME

sympa, sympa.pl - Command line utility to manage Sympa

# SYNOPSIS

`sympa.pl` \[ `-d, --debug` \] \[ `-f, --file`=_another.sympa.conf_ \]
\[ `-l, --lang`=_lang_ \] \[ `-m, --mail` \]
\[ `-h, --help` \] \[ `-v, --version` \]

\[ `--import`=_listname_ \]
\[ `--open_list`=_list_\[_@robot_\] \]
\[ `--close_list`=_list_\[_@robot_\] \]
\[ `--purge_list`=_list_\[_@robot_\] \]
\[ `--lowercase` \] \[ `--make_alias_file` \]
\[ `--dump_users` `--list`=_list_@_domain_|ALL \[ `--role`=_roles_ \] \]
\[ `--restore_users` `--list`=_list_@_domain_|ALL \[ `--role`=_roles_ \] \]

# DESCRIPTION

NOTE:
On overview of Sympa documentation see [sympa\_toc(1)](./sympa_toc.1.md).

Sympa.pl is invoked from command line then performs various administration
tasks.

# OPTIONS

`sympa.pl` may run with following options in general.

- `-d`, `--debug`

    Enable debug mode.

- `-f`, `--config=`_file_

    Force Sympa to use an alternative configuration file instead
    of `/etc/sympa/sympa.conf`.

- `-l`, `--lang=`_lang_

    Set this option to use a language for Sympa. The corresponding
    gettext catalog file must be located in `$LOCALEDIR`
    directory.

- `--log_level=`_level_

    Sets Sympa log level.

With the following options `sympa.pl` will run in batch mode:

- `--add_list=`_family\_name_ `--robot=`_robot\_name_
`--input_file=`_/path/to/file.xml_

    Add the list described by the file.xml under robot\_name, to the family
    family\_name.

- `--change_user_email` `--current_email=`_xx_ `--new_email=`_xx_

    Changes a user email address in all Sympa  databases (subscriber\_table,
    list config, etc) for all virtual robots.

- `--close_family=`_family\_name_ `--robot=`_robot\_name_

    Close lists of family\_name family under robot\_name.      

- `--close_list=`_list_\[_@robot_\]

    Close the list (changing its status to closed), remove aliases and remove
    subscribers from DB (a dump is created in the list directory to allow
    restoring the list)

- `--conf_2_db`

    Load sympa.conf and each robot.conf into database.

- `--create_list` `--robot=`_robot\_name_
`--input_file=`_/path/to/file.xml _

    Create a list with the XML file under robot robot\_name.

- `--dump=`_list_@_domain_|`ALL`

    Obsoleted option.  Use `--dump_users`.

- `--dump_users` `--list=`_list_@_domain_|`ALL` \[ `--role=`_roles_ \]

    Dumps users of a list or all lists.

    `--role` may specify `member`, `owner`, `editor` or any of them separated
    by comma (`,`). Only `member` is chosen by default.

    Users are dumped in files _role_`.dump` in each list directory.

    Note: On Sympa prior to 6.2.31b.1, subscribers were dumped in
    `subscribers.db.dump` file, and owners and moderators could not be dumped.

    See also `--restore_users`.

    Note: This option replaced `--dump` on Sympa 6.2.34.

- `--health_check`

    Check if `sympa.conf`, `robot.conf` of virtual robots and database structure
    are correct.  If any errors occur, exits with non-zero status.

- `--import=`_list_@_dom_

    Import subscribers in the list. Data are read from standard input.
    The imported data should contain one entry per line : the first field
    is an email address, the second (optional) field is the free form name.
    Fields are spaces-separated.

    Sample:

        ## Data to be imported
        ## email        gecos
        john.steward@some.company.com           John - accountant
        mary.blacksmith@another.company.com     Mary - secretary

- `--instantiate_family=`_family\_name_ `--robot=`_robot\_name_
`--input_file=`_/path/to/file.xml_ \[ `--close_unknown` \] \[ `--quiet` \]

    Instantiate family\_name lists described in the file.xml under robot\_name.
    The family directory must exist; automatically close undefined lists in a
    new instantiation if --close\_unknown is specified; do not print report if
    `--quiet` is specified.

- `--lowercase`

    Lowercases email addresses in database.

- `--make_alias_file` \[ `--robot` robot \]

    Create an aliases file in /tmp/ with all list aliases. It uses the
    `list_aliases.tt2` template  (useful when list\_aliases.tt2 was changed).

- `--md5_encode_password`

    Rewrite password in `user_table` of database using MD5 fingerprint.
    YOU CAN'T UNDO unless you save this table first.

    **Note** that this option was obsoleted.
    Use [upgrade\_sympa\_password(1)](./upgrade_sympa_password.1.md).

- `--modify_list=`_family\_name_ `--robot=`_robot\_name_
`--input_file=`_/path/to/file.xml_

    Modify the existing list installed under the robot robot\_name and that
    belongs to the family family\_name. The new description is in the `file.xml`.

- `--open_list=`_list_\[_@robot_\]

    Restore the closed list (changing its status to open), add aliases and restore
    users to DB (dump files in the list directory are imported).

- `--purge_list`=_list_\[@_robot_\]

    Remove the list (remove archive, configuration files, users and owners in admin table. Restore is not possible after this operation.

- `--reload_list_config`
\[ `--list=`_mylist_@_mydom_ \] \[ `--robot=`_mydom_ \]

    Recreates all `config.bin` files or cache in `list_table`.
    You should run this command if you edit authorization scenarios.
    The list and robot parameters are optional.

- `--rename_list=`_listname_@_robot_
`--new_listname=`_newlistname_ `--new_listrobot=`_newrobot_

    Renames a list or move it to another virtual robot.

- `--send_digest` \[ `--keep_digest` \]

    Send digest right now.
    If `--keep_digest` is specified, stocked digest will not be removed.

- `--restore_users` `--list=`_list_@_domain_|`ALL` \[ `--role=`_roles_ \]

    Restore users from files dumped by `--dump_users`.

    Note: This option was added on Sympa 6.2.34.

- `--sync_include=`_listname_@_robot_

    Trigger the list members update.

- `--sync_list_db` \[ `--list=`_listname_@_robot_ \]

    Syncs filesystem list configs to the database cache of list configs,
    optionally syncs an individual list if specified.

- `--test_database_message_buffer`

    **Note**:
    This option was deprecated.

    Test the database message buffer size.

- `--upgrade` \[ `--from=`_X_ \] \[ `--to=`_Y_ \]

    Runs Sympa maintenance script to upgrade from version _X_ to version _Y_.

- `--upgrade_shared` \[ `--list=`_X_ \] \[ `--robot=`_Y_ \]

    **Note**:
    This option was deprecated.
    See upgrade\_shared\_repository(1).

    Rename files in shared.

With following options `sympa.pl` will print some information and exit.

- `-h`, `--help`

    Print this help message.

- `--md5_digest=`_password_

    Output a MD5 digest of a password (useful for SOAP client trusted
    application).

- `-v`, `--version`

    Print the version number.

# FILES

`/etc/sympa/sympa.conf` main configuration file.

# SEE ALSO

[sympa\_toc(1)](./sympa_toc.1.md).

# HISTORY

This program was originally written by:

- Serge Aumont

    Comité Réseau des Universités

- Olivier Salaün

    Comité Réseau des Universités

As of Sympa 6.2b.4, it was split into three programs:
`sympa.pl` command line utility, `sympa_automatic.pl` daemon and
`sympa_msg.pl` daemon.
