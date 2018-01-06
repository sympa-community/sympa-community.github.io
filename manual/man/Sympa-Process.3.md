---
title: 'Sympa::Process(3)'
---

# NAME

Sympa::Process - Process of Sympa

# SYNOPSIS

    use Sympa::Process;
    my $process = Sympa::Process->instance;
    $process->init(pidname => 'sympa');

    $process->daemonize;

    $process->fork;

# DESCRIPTION

[Sympa::Process](./Sympa-Process.3.md) implements the class to handle process itself of Sympa
software.

## Signal handling

Once [Sympa::Process](./Sympa-Process.3.md) is loaded,
`SIGCHLD` signals are captured,
and only defunct child processes invoked by fork() method are reaped.

## Methods

- instance ( )

    _Constructor_.
    Creates a singleton instance of [Sympa::Process](./Sympa-Process.3.md) object.

    Returns:

    A new [Sympa::Process](./Sympa-Process.3.md) instance, or _undef_ for failure.

- init ( key => value, ... )

    _Instance method_.
    TBD.

- daemonize ( )

    _Instance method_.
    Daemonizes process itself.
    Process is given new process group, detached from TTY
    and given new process ID.

    Parameters:

    None.

    Returns:

    None.

- fork ( \[ $tag \] )

    _Instance method_.
    Forks process.
    Note that this method should be used instead of fork() in Perl core.

    Parameter:

    - $tag

        A string to determine new child process.
        By default the name of calling process.

    Returns:

    See ["fork" in perlfunc](https://metacpan.org/pod/perlfunc#fork).

- reap\_child ( \[ blocking => 1 \] )

    DEPRECATED.

- wait\_child ( )

    _Instance method_.
    Waits for any child process.

    Parameters:

    None.

    Returns:

    `0`.
    Returns `-1` on failure.

- sync\_child ( \[ hash => \\%hash \], \[ file => 1 \] )

    Updates process information in external data.

    Parameters:

    - hash => \\%hash

        Syncs PIDs in local map %hash

    - file => 1

        Syncs child PIDs in PID file.
        If dead PID is found, notification will be sent to super-listmaster.

    Returns:

    None.

- remove\_pid (\[ pid => $pid \], \[ final => 1 \] )

    _Instance method_.
    Removes process ID from PID file.
    Then if the file is empty, it will be removed.

- write\_pid ( \[ initial => 1 \], \[ pid => $pid \] )

    _Instance method_.
    Writes or adds process ID to PID file.

    Parameters:

    - initial => 1

        Initializes PID file.
        If the file remains, notification will be sent to super-listmaster.

    - pid => $pid

        Process ID to be written.
        By default PID of current process.

- direct\_stderr\_to\_file ( )

    _Instance method_.
    TBD.

## Attributes

[Sympa::Process](./Sympa-Process.3.md) instance may have following attributes:

- {children}

    Hashref with child PIDs forked by fork() method as keys.

- {detached}

    True value is set if daemonize() method was called and the process has been
    detached from TTY.

- {generation}

    Generation of process.
    If fork() method succeeds, it will be increased by child process.

## Utility functions

- eval\_in\_time ( $subref, $timeout )

    Evaluate subroutine $subref in $timeout seconds.

    TBD.

- register\_handler ( )

    Registers `SIGCHLD` handler.
    This function is usually called automatically during initialization.

# HISTORY

[Sympa::Tools::Daemon](./Sympa-Tools-Daemon.3.md) appeared on Sympa 6.2a.41.

Renamed [Sympa::Process](./Sympa-Process.3.md) appeared on Sympa 6.2.12
and began to provide OO interface.

Sympa 6.2.13 introduced daemonize() method and {detached} attribute.

As of Sympa 6.2.14, `SIGCHLD` signal was captured and child processes
were reaped immediately.  reap\_child() method (formerly reaper()) was
deprecated.
