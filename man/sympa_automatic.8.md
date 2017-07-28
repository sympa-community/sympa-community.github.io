# NAME

sympa\_automatic, sympa\_automatic.pl - Automatic list creation daemon

# SYNOPSIS

**sympa\_automatic.pl** \[ **-d, --debug** \] \[ **-f, --file**=_another.sympa.conf_ \]
      \[ **-k, --keepcopy**=_directory_ \]
      \[ \[ **-m, --mail** \]
      \[ **-h, --help** \] \[ **-v, --version** \]

# DESCRIPTION

Sympa\_automatic.pl is a program which scans permanently the automatic creation
spool and processes each message.

If the list a message is bound for has not been there and list creation is
authorized, it will be created.  Then the message is stored into incoming
message spool again and wait for processing by `sympa_msg.pl`.

# OPTIONS

`sympa_automatic.pl` may run with following options in general.

- **-d**, **--debug**

    Enable debug mode.

- **-f**, **--config=**_file_

    Force Sympa to use an alternative configuration file instead
    of `/etc/sympa/sympa.conf`.

- **--log\_level=**_level_

    Sets Sympa log level.

`sympa_automatic.pl` may run in daemon mode with following options.

- **--foreground**

    The process remains attached to the TTY.

- **-k**, **--keepcopy=**`directory`

    This option tells Sympa to keep a copy of every incoming message, 
    instead of deleting them. \`directory' is the directory to 
    store messages.

- **-m**, **--mail**

    Sympa will log calls to sendmail, including recipients. This option is
    useful for keeping track of each mail sent (log files may grow faster
    though).

With following options `sympa_automatic.pl` will print some information and exit.

- **-h**, **--help**

    Print this help message.

- **-v**, **--version**

    Print the version number.

# FILES

`/etc/sympa/sympa.conf` main configuration file.

`/home/sympa/sympa_automatic.pid` this file contains the process ID
of `sympa_automatic.pl`.

# SEE ALSO

[sympa.conf(5)](./sympa.conf.5.md), [sympa\_msg(8)](./sympa_msg.8.md).

[Sympa::Spindle::ProcessAutomatic](./Sympa::Spindle::ProcessAutomatic.3.md).

# HISTORY

`sympa.pl` was originally written by:

- Serge Aumont

    Comité Réseau des Universités

- Olivier Salaün

    Comité Réseau des Universités

As of Sympa 6.2b.4, it was split into three programs:
`sympa.pl` command line utility, `sympa_automatic.pl` daemon and
`sympa_msg.pl` daemon.
