# NAME

sympa\_newaliases, sympa\_newaliases.pl - Alias database maintenance

# SYNOPSIS

    sympa_newaliases.pl --domain=dom.ain

# DESCRIPTION

sympa\_newaliases is a program to maintain alias database.

It is typically called by
[alias\_manager(8)](./alias_manager.8.md) via sympa\_newaliases-wrapper,
then updates alias database.

# OPTIONS

`sympa_newaliases.pl` may run with following options.

- **--domain=**_domain_

    Name of virtual robot on which aliases will be updated.

- **-f**, **--config=**_file_

    Force sympa\_newaliases to use an alternative configuration file instead
    of `/etc/sympa/sympa.conf`.

- **-h**, **--help**

    Print this help message.

# CONFIGURATION PARAMETERS

Following site configuration parameters in `/etc/sympa/sympa.conf` will be referred.
They may be overridden by robot.conf of each virtual robot.

- sendmail\_aliases

    Source text of alias database.

    Default value is `/etc/mail/sympa_aliases`.

- aliases\_program

    System command to update alias database.
    Possible values are:

    - `makemap`

        Sendmail makemap utility.

    - `newaliases`

        [newaliases(1)](./newaliases.1.md) or compatible utility.

    - `postalias`

        Postfix [postalias(1)](./postalias.1.md) utility.

    - `postmap`

        Postfix [postmap(1)](./postmap.1.md) utility.

    - Full path

        Full path to executable file.
        File will be invoked with the value of `sendmail_aliases` as an argument.

    Default value is `newaliases`.

- aliases\_db\_type

    Type of alias database.
    This is meaningful when value of `aliases_program` parameter is
    `makemap`, `postalias` or `postmap`.

    Possible values will be vary by system commands.
    For example, `postalias` and `postmap` can support any of
    `btree`, `cdb`, `dbm`, `hash` and `sdbm`.

    Default value is `hash`.

# RETURN VALUE

Returns with exit code 0.
If invoked system command failed, returns with its exit code.
On other failures, returns with 1.

# FILES

- `/etc/sympa/sympa.conf`

    Sympa site configuration.

- `/home/sympa/bin/sympa_newaliases-wrapper`

    Set UID wrapper for sympa\_newaliases.pl.

# HISTORY

sympa\_newaliases.pl appeared on Sympa 6.1.18.
It was initially written by
IKEDA Soji <ikeda@conversion.co.jp>.

# SEE ALSO

[alias\_manager(8)](./alias_manager.8.md).
