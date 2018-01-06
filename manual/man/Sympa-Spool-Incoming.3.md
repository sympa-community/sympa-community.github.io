---
title: 'Sympa::Spool::Incoming(3)'
---

# NAME

Sympa::Spool::Incoming - Spool for incoming messages

# SYNOPSIS

    use Sympa::Spool::Incoming;
    my $spool = Sympa::Spool::Incoming->new;

    $spool->store($message);

    my ($message, $handle) = $spool->next;

# DESCRIPTION

[Sympa::Spool::Incoming](./Sympa-Spool-Incoming.3.md) implements the spool for incoming messages.

## Methods

See also ["Public methods" in Sympa::Spool](./Sympa-Spool.3.md#public-methods).

- next ( \[ no\_filter => 1 \], \[ no\_lock => 1 \] )

    Order is controlled by modification time of file and delivery date, then,
    if `no_filter` is _not_ set,
    messages with possiblly higher priority are chosen and
    messages with lowest priority (`z` or `Z`) are skipped.

- store ( $message, \[ original => $original \] )

    In most cases, queue(8) program stores messages to incoming spool.
    Daemon such as sympa\_automatic(8) uses this method to store messages.

## Context and metadata

See also ["Marshaling and unmarshaling metadata" in Sympa::Spool](./Sympa-Spool.3.md#marshaling-and-unmarshaling-metadata).

This class particularly gives following metadata:

- {date}

    Unix time when the message would be delivered.

# CONFIGURATION PARAMETERS

Following site configuration parameters in sympa.conf will be referred.

- queue

    Directory path of incoming spool.

# SEE ALSO

[sympa\_automatic(8)](./sympa_automatic.8.md), [sympa\_msg(8)](./sympa_msg.8.md), [Sympa::Message](./Sympa-Message.3.md), [Sympa::Spool](./Sympa-Spool.3.md).

# HISTORY

[Sympa::Spool::Incoming](./Sympa-Spool-Incoming.3.md) appeared on Sympa 6.2.5.
