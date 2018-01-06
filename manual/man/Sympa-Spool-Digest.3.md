---
title: 'Sympa::Spool::Digest(3)'
---

# NAME

Sympa::Spool::Digest - Spool for messages waiting for digest sending

# SYNOPSIS

    use Sympa::Spool::Digest;
    my $spool = Sympa::Spool::Digest->new(context => $list);
    
    $spool->store($message);
    
    my ($message, $handle) = $spool->next;

# DESCRIPTION

[Sympa::Spool::Digest](./Sympa-Spool-Digest.3.md) implements the spool for messages waiting for
digest sending.

## Methods

See also ["Public methods" in Sympa::Spool](./Sympa-Spool.3.md#public-methods).

- new ( context => $list )

    Creates new instance of [Sympa::Spool::Digest](./Sympa-Spool-Digest.3.md) related to the list $list.

- next ( )

    Order is controlled by delivery date, then by reception date.

## Properties

See also ["Properties" in Sympa::Spool](./Sympa-Spool.3.md#properties).

- {time}

    Earliest time of messages in the spool, or `undef`.

## Context and metadata

See also ["Marshaling and unmarshaling metadata" in Sympa::Spool](./Sympa-Spool.3.md#marshaling-and-unmarshaling-metadata).

This class particularly gives following metadata:

- {date}

    Unix time when the message was delivered.

- {time}

    Unix time in floating point number when the message was stored.

# CONFIGURATION PARAMETERS

Following site configuration parameters in sympa.conf will be referred.

- queuedigest

    Parent directory path of digest spools.

# SEE ALSO

[sympa\_msg(8)](./sympa_msg.8.md),
[Sympa::Message](./Sympa-Message.3.md), [Sympa::Spool](./Sympa-Spool.3.md), [Sympa::Spool::Digest::Collection](./Sympa-Spool-Digest-Collection.3.md).

# HISTORY

[Sympa::Spool::Digest](./Sympa-Spool-Digest.3.md) appeared on Sympa 6.2.6.
