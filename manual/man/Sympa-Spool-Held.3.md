# NAME

Sympa::Spool::Held - Spool for held messages waiting for confirmation

# SYNOPSIS

    use Sympa::Spool::Held;

    my $spool = Sympa::Spool::Held->new;
    my $authkey = $spool->store($message);

    my $spool =
        Sympa::Spool::Held->new(context => $list, authkey => $authkey);
    my ($message, $handle) = $spool->next;

# DESCRIPTION

[Sympa::Spool::Held](./Sympa-Spool-Held.3.md) implements the spool for held messages waiting for
confirmation.

## Methods

See also ["Public methods" in Sympa::Spool](./Sympa-Spool.3.md#public-methods).

- new ( \[ context => $list \], \[ authkey => $authkey \] )
- next ( \[ no\_lock => 1 \] )

    If the pairs describing metadatas are specified,
    contents returned by next() are filtered by them.

- quarantine ( )

    Does nothing.

- store ( $message, \[ original => $original \] )

    If storing succeeded, returns authentication key.

## Context and metadata

See also ["Marshaling and unmarshaling metadata" in Sympa::Spool](./Sympa-Spool.3.md#marshaling-and-unmarshaling-metadata).

This class particularly gives following metadata:

- {authkey}

    Authentication key generated automatically
    when the message is stored to spool.

# CONFIGURATION PARAMETERS

Following site configuration parameters in sympa.conf will be referred.

- queueauth

    Directory path of held message spool.

    Note:
    Named such by historical reason.

# SEE ALSO

[sympa\_msg(8)](./sympa_msg.8.md), [wwsympa(8)](./wwsympa.8.md),
[Sympa::Message](./Sympa-Message.3.md), [Sympa::Spool](./Sympa-Spool.3.md).

# HISTORY

[Sympa::Spool::Held](./Sympa-Spool-Held.3.md) appeared on Sympa 6.2.8.
