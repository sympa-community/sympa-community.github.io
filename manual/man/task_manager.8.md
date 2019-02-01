---
title: 'task_manager(8)'
---

# NAME

task\_manager, task\_manager.pl - Daemon to process periodical Sympa tasks

# SYNOPSIS

`task_manager.pl` \[ `--foreground` \] \[ `--debug` \]

# DESCRIPTION

Task\_manager is a program which scans permanently the task spool and
processes tasks.
It also checks configuration of site and every list to create necessary tasks.

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

# SEE ALSO

[sympa\_msg(8)](./sympa_msg.8.md), [wwsympa(8)](./wwsympa.8.md).

[Sympa::Spindle::ProcessTask](./Sympa-Spindle-ProcessTask.3.md).

# HISTORY

`task_manager.pl` was contributed by F. Guilleux.
It appeared on Sympa 3.3a-vhost.10.
