# NAME

Sympa::Tools::Time - Time-related functions

# DESCRIPTION

This package provides some time-related functions.

## Functions

- date\_conv ( $arg )

    _Function_.
    TBD.

- duration\_conv ( $arg, \[ $startdate \] )

    _Function_.
    TBD.

- epoch\_conv ( $arg )

    _Function_.
    Converts a human format date into an Unix time.
    TBD.

- get\_midnight\_time ( $time )

    _Function_.
    Returns the Unix time corresponding to the last midnight before date given
    as argument.

- gettimeofday ( )

    _Function_.
    Returns an array `(_second_, _microsecond_)` of current Unix time.

    If the system does not have gettimeofday(2) system call, this function
    emulates it.

# HISTORY

[Sympa::Tools::Time](./Sympa::Tools::Time.3.md) appeared on Sympa 6.2a.37.

gettimeofday() function was introduced on Sympa 6.2.10.
