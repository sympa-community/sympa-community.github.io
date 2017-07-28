# NAME

Sympa::Spool::Archive - Spool for messages waiting for archiving

# SYNOPSIS

    use Sympa::Spool::Archive;
    my $spool = Sympa::Spool::Archive->new;

    $spool->store($message);

    my ($message, $handle) = $spool->next;

# DESCRIPTION

[Sympa::Spool::Archive](./Sympa::Spool::Archive.3.md) implements the spool for messages waiting for
archiving.

## Methods

See also ["Public methods" in Sympa::Spool](./Sympa::Spool.3.md#public-methods).

- next ( )

    Order is controlled by delivery date, then by reception date.

## Context and metadata

See also ["Marshaling and unmarshaling metadata" in Sympa::Spool](./Sympa::Spool.3.md#marshaling-and-unmarshaling-metadata).

This class particularly gives following metadata:

- {date}

    Unix time when the message would be delivered.

- {time}

    Unix time in floating point number when the message was stored.

# CONFIGURATION PARAMETERS

Following site configuration parameters in sympa.conf will be referred.

- queueoutgoing

    Directory path of archive spool.

    Note:
    Named such by historical reason.

# SEE ALSO

[Sympa::Archive](./Sympa::Archive.3.md), [Sympa::Message](./Sympa::Message.3.md), [Sympa::Spindle::ProcessArchive](./Sympa::Spindle::ProcessArchive.3.md),
[Sympa::Spool](./Sympa::Spool.3.md).

# HISTORY

[Sympa::Spool::Archive](./Sympa::Spool::Archive.3.md) appeared on Sympa 6.2.
