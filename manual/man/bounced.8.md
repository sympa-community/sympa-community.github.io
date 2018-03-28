---
title: 'bounced(8)'
---

# NAME

bounced, bounced.pl - Mailing List Bounce Processing Daemon for Sympa

# SYNOPSIS

`bounced.pl` \[ `--foreground` \] \[ `--debug` \]

# DESCRIPTION

Bounced is a program which scans permanently the bounce spool and
processes bounces (non-delivery messages), looking or bad addresses.
Bouncing addresses are tagged in database ; last bounce is kept for
each bouncing address.

List owners will latter access bounces information via WWSympa.

# OPTIONS

These programs follow the usual GNU command line syntax,
with long options starting with two dashes (`--`).  A summary of
options is included below.

- `-F`, `--foreground`

    Do not detach TTY.

- `-f`, `--config=`_file_

    Force bounced to use an alternative configuration file instead
    of `/etc/sympa/sympa.conf`.

- `-d`, `--debug`

    Run the program in a debug mode.

- `-h`, `--help`

    Print this help message.

- `--log_level=`_level_

    Sets daemon log level.

# FILES

`/etc/sympa/sympa.conf` Sympa configuration file.

`$LIBEXECDIR/bouncequeue` bounce spooler, referenced from sendmail alias file

`$SPOOLDIR/bounce` incoming bounces directory

`$PIDDIR/bounced.pid` this file contains the process ID
of `bounced.pl`.

# MORE DOCUMENTATION

The full documentation can be
found in [https://sympa-community.github.io/manual/](https://sympa-community.github.io/manual/).

The mailing lists (with web archives) can be accessed at
[https://listes.renater.fr/sympa/lists/informatique/sympa](https://listes.renater.fr/sympa/lists/informatique/sympa).

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

[sympa\_msg(8)](./sympa_msg.8.md), [wwsympa(8)](./wwsympa.8.md), [mhonarc(1)](./mhonarc.1.md), [sympa.conf(5)](./sympa.conf.5.md).

[Sympa::Spindle::ProcessBounce](./Sympa-Spindle-ProcessBounce.3.md).
