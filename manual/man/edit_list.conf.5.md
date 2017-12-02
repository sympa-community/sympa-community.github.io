# NAME

edit\_list.conf - Configuration of privileges to edit list configuration

# DESCRIPTION

`edit_list.conf` defines privileges to edit list configuration.

`$SYSCONFDIR/edit_list.conf` is main configuration.
Several parameters defined in this file may be overridden by
`$SYSCONFDIR/<mail domain name>/edit_list.conf`
file for each mail domain, or by
`<list config directory>/edit_list.conf` file for each mailing list.

Format of `edit_list.conf` is as following:

- Lines beginning with `#` and containing only spaces are ignored.
- Each line has the form "_parameter name_ _role_ _privilege_".

_parameter name_ is a list parameter name (e.g. `max_size`),
paragraph name (`owner`) or subparameter (`owner.email)`).
It may be a name of template (e.g. `welcome.tt2`) or list configuration file
(`message.header`) except main configuration file (`config`).

_role_ is any of `listmaster`, `privileged_owner`, `owner` or `editor`.
Multiple roles have to be separated by comma ("`,`").

_privilege_ is `write`, `read` or `hidden`.

Lines are checked in order, then matched line at the first time wins.
Default privilege may be specified with special parameter name `default`.

# FILES

- `$DEFAULTDIR/edit_list.conf`

    Distribution default.  This file should not be edited.

- `$SYSCONFDIR/edit_list.conf`
- `$SYSCONFDIR/<mail domain name>/edit_list.conf`
- `$EXPLDIR/<list name>/edit_list.conf` or
`$EXPLDIR/<mail domain name>/<list name>/edit_list.conf`

    Configuration files for site-wide default, by each domain or each list.

# SEE ALSO

[wwsympa(8)](./wwsympa.8.md).

# HISTORY

This document was initially written by IKEDA Soji <ikeda@conversion.co.jp>.
