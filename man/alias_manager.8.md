# NAME

alias\_manager, alias\_manager.pl - Manage Sympa Aliases

# SYNOPSIS

**alias\_manager.pl** **add** | **del** _listname_ _domain_

# DESCRIPTION

Alias\_manager is a program that helps in installing aliases for newly
created lists and deleting aliases for closed lists. 

It is called by
[wwsympa.fcgi(8)](./wwsympa.8.md) or [sympa.pl(8)](./sympa.8.md) via the _aliaswrapper_.
Alias management is performed only if it was setup in `/etc/sympa/sympa.conf`
(`sendmail_aliases` configuration parameter).

Administrators using MTA functionalities to manage aliases (ie
virtual\_regexp and transport\_regexp with postfix) can disable alias
management by setting
`sendmail_aliases` configuration parameter to **none**.

# OPTIONS

- **add** _listname_ _domain_

    Add the set of aliases for the mailing list _listname_ in the
    domain _domain_.

- **del** _listname_ _domain_

    Remove the set of aliases for the mailing list _listname_ in the
    domain _domain_.

# FILES

`$SENDMAIL_ALIASES` sendmail aliases file.

# DOCUMENTATION

The full documentation in HTML and PDF formats can be
found in [http://www.sympa.org/manual/](http://www.sympa.org/manual/). 

The mailing lists (with web archives) can be accessed at
http://listes.renater.fr/sympa/lists/informatique/sympa.

# HISTORY

This program was originally written by:

- Serge Aumont

    Comité Réseau des Universités

- Olivier Salaün

    Comité Réseau des Universités

This manual page was initially written by
Jérôme Marant &lt;jerome.marant@IDEALX.org>
for the Debian GNU/Linux system.

# LICENSE

You may distribute this software under the terms of the GNU General
Public License Version 2.  For more details see `README` file.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.1 or
any later version published by the Free Software Foundation; with no
Invariant Sections, no Front-Cover Texts and no Back-Cover Texts.  A
copy of the license can be found under
[http://www.gnu.org/licenses/fdl.html](http://www.gnu.org/licenses/fdl.html).

# BUGS

Report bugs to Sympa bug tracker.
See [http://www.sympa.org/tracking](http://www.sympa.org/tracking).

# SEE ALSO

[sympa(1)](./sympa.1.md), [sympa\_msg(8)](./sympa_msg.8.md), [sendmail(8)](./sendmail.8.md), [wwsympa(8)](./wwsympa.8.md).
