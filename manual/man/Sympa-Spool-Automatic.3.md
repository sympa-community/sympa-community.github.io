# NAME

Sympa::Spool::Automatic - Spool for incoming messages in automatic spool

# SYNOPSIS

    use Sympa::Spool::Automatic;
    my $spool = Sympa::Spool::Automatic->new;

    my ($message, $handle) = $spool->next;

# DESCRIPTION

[Sympa::Spool::Automatic](./Sympa-Spool-Automatic.3.md) implements the spool for incoming messages in
automatic spool.

## Methods

See also ["Public methods" in Sympa::Spool](./Sympa-Spool.3.md#public-methods).

- next ( \[ no\_filter => 1 \], \[ no\_lock => 1 \] )

    _Instance method_.
    Order is controlled by modification time of files and delivery date, then,
    if `no_filter` is _not_ set,
    messages with possiblly higher priority are chosen and
    messages with lowest priority (`z` or `Z`) are skipped.

- store ( $message, \[ original => $original \] )

    In most cases, familyqueue(8) program stores messages to automatic spool.
    This method is not used in ordinal case.

## Context and metadata

See also ["Marshaling and unmarshaling metadata" in Sympa::Spool](./Sympa-Spool.3.md#marshaling-and-unmarshaling-metadata).

This class particularly gives following metadata:

- {date}

    Unix time when the message would be delivered.

# CONFIGURATION PARAMETERS

Following site configuration parameters in sympa.conf will be referred.

- queueautomatic

    Directory path of list creation spool.

# SEE ALSO

[sympa\_automatic(8)](./sympa_automatic.8.md), [Sympa::Message](./Sympa-Message.3.md), [Sympa::Spool](./Sympa-Spool.3.md).

# HISTORY

[Sympa::Spool::Automatic](./Sympa-Spool-Automatic.3.md) appeared on Sympa 6.2.6.
