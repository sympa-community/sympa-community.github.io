# NAME

Sympa::Log - Logging facility of Sympa

# SYNOPSIS

    use Sympa::Log;

    my $log = Sympa::Log->instance;
    $log->openlog($facility, 'inet');
    $log->{level} = 0;
    $log->syslog('info', '%s: Stat logging', $$);

# DESCRIPTION

TBD.

## Methods

- instance ( )

    _Constructor_.
    Creates new singleton instance of [Sympa::Log](./Sympa-Log.3.md).

- openlog ( $facility, $socket\_type, \[ options ... \] )

    TBD.

- syslog ( $level, $format, \[ parameters ... \] )

    TBD.

- get\_log\_date

    TBD,

- db\_log

    TBD.

- add\_stat

    TBD.

- get\_first\_db\_log

    TBD.

- get\_next\_db\_log

    TBD.

- aggregate\_stat

    TBD.

- aggregate\_daily\_data

    TBD.

## Properties

Instance of [Sympa::Log](./Sympa-Log.3.md) has following properties.

- {level}

    Logging level.  Integer or `undef`.

- {log\_to\_stderr}

    If set, print logs by syslog() to standard error.
    Property value may be log level(s) to print or `'all'`.

# SEE ALSO

[Sys::Syslog](https://metacpan.org/pod/Sys::Syslog), [Sympa::DatabaseManager](./Sympa-DatabaseManager.3.md).

# HISTORY

Database logging feature contributed by Adrien Brard appeared on Sympa 5.3.

Statistics feature appeared on Sympa 6.2.

[Log](https://metacpan.org/pod/Log) module was renamed to [Sympa::Log](./Sympa-Log.3.md) on Sympa 6.2.
