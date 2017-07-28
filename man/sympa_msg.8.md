# NAME

sympa\_msg, sympa\_msg.pl - Daemon to handle incoming messages

# SYNOPSIS

**sympa\_msg.pl** \[ **-d, --debug** \] \[ **-f, --file**=_another.sympa.conf_ \]
      \[ **-k, --keepcopy**=_directory_ \]
      \[ **-l, --lang**=_lang_ \] \[ **-m, --mail** \]
      \[ **-h, --help** \] \[ **-v, --version** \]

# DESCRIPTION

Sympa\_msg.pl is a program which scans permanently the incoming message spool
and processes each message.

Messages bound for the lists and authorized sending are modified as neccesity
and at last stored into digest spool, archive spool and outgoing spool.
Those bound for command addresses are interpreted and appropriate actions are
taken.
Those bound for listmasters or list admins are forwarded to them.

# OPTIONS

Sympa\_msg.pl follows the usual GNU command line syntax,
with long options starting with two dashes (`--`).  A summary of
options is included below.

- **-d**, **--debug**

    Enable debug mode.

- **-f**, **--config=**_file_

    Force Sympa to use an alternative configuration file instead
    of `/etc/sympa/sympa.conf`.

- **-l**, **--lang=**_lang_

    Set this option to use a language for Sympa. The corresponding
    gettext catalog file must be located in `/home/sympa/locale`
    directory.

- **--log\_level=**_level_

    Sets Sympa log level.

`sympa_msg.pl` may run in daemon mode with following options.

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

- **--service=process\_command**|**process\_message**|**process\_creation**

    **Note**:
    This option was deprecated.

    Process is dedicated to messages distribution, commands or to automatic lists
    creation (default three of them).

With following options `sympa_msg.pl` will print some information and exit.

- **-h**, **--help**

    Print this help message.

- **-v**, **--version**

    Print the version number.

# FILES

`/etc/sympa/sympa.conf` main configuration file.

`/home/sympa/sympa_msg.pid` this file contains the process ID
of `sympa_msg.pl`.

# SEE ALSO

[sympa.conf(5)](./sympa.conf.5.md), [sympa(1)](./sympa.1.md).

[archived(8)](./archived.8.md), [bulk(8)](./bulk.8.md), [bounced(8)](./bounced.8.md), [sympa\_automatic(8)](./sympa_automatic.8.md),
[task\_manager(8)](./task_manager.8.md).

[Sympa::Spindle::ProcessDigest](./Sympa-Spindle-ProcessDigest.3.md),
[Sympa::Spindle::ProcessIncoming](./Sympa-Spindle-ProcessIncoming.3.md).

# HISTORY

`sympa.pl` was originally written by:

- Serge Aumont

    Comité Réseau des Universités

- Olivier Salaün

    Comité Réseau des Universités

As of Sympa 6.2b.4, it was split into three programs:
`sympa.pl` command line utility, `sympa_automatic.pl` daemon and
`sympa_msg.pl` daemon.
