# NAME

task\_manager, task\_manager.pl - Daemon to Process Periodical Sympa Tasks

# SYNOPSIS

`task_manager.pl` \[ `--foreground` \] \[ `--debug` \]

# DESCRIPTION

XXX @todo doc

# OPTIONS

- `-d`, `--debug`

    Sets the debug mode

- `-f`, `--config=`_file_

    Force task\_manager to use an alternative configuration file instead
    of `/etc/sympa/sympa.conf`.

- `-F`, `--foreground`

    Prevents the script from being daemonized

- `-h`, `--help`

    Prints this help message.

- `--log_level=`_level_

    Set log level.

# FILES

`$SPOOLDIR/task/` directory for task spool.

`$PIDDIR/task_manager.pid` this file contains the process ID
of `task_manager.pl`.

# MORE DOCUMENTATION

The full documentation in HTML and PDF formats can be
found in [http://www.sympa.org/manual/](http://www.sympa.org/manual/).

The mailing lists (with web archives) can be accessed at
[http://listes.renater.fr/sympa/lists/informatique/sympa](http://listes.renater.fr/sympa/lists/informatique/sympa).

# BUGS

Report bugs to Sympa bug tracker.
See [http://www.sympa.org/tracking](http://www.sympa.org/tracking).

# SEE ALSO

[sympa\_msg(8)](./sympa_msg.8.md), [wwsympa(8)](./wwsympa.8.md)
