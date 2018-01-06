---
title: 'Sympa::Spool::Bounce(3)'
---

# NAME

Sympa::Spool::Bounce - Spool for incoming bounce messages

# SYNOPSIS

    use Sympa::Spool::Bounce;
    my $spool = Sympa::Spool::Bounce->new;
    
    my ($message, $handle) = $spool->next;

# DESCRIPTION

[Sympa::Spool::Bounce](./Sympa-Spool-Bounce.3.md) implements the spool for incoming bounce messages.

## Methods

See also ["Public methods" in Sympa::Spool](./Sympa-Spool.3.md#public-methods).

- next ( )

    Order is controlled by modification time of files and delivery date.

- store ( $message, \[ original => $original \] )

    In most cases, bouncequeue(8) program stores messages to bounce spool.
    This method is not used in ordinal case.

## Context and metadata

See also ["Marshaling and unmarshaling metadata" in Sympa::Spool](./Sympa-Spool.3.md#marshaling-and-unmarshaling-metadata).

This class particularly gives following metadata:

- {date}

    Unix time when the message would be delivered.

# CONFIGURATION PARAMETERS

Following site configuration parameters in sympa.conf will be referred.

- queuebounce

    Directory path of bounce spool.

# SEE ALSO

[bounced(8)](./bounced.8.md), [Sympa::Message](./Sympa-Message.3.md), [Sympa::Spool](./Sympa-Spool.3.md), [Sympa::Tracking](./Sympa-Tracking.3.md).

# HISTORY

[Sympa::Spool::Bounce](./Sympa-Spool-Bounce.3.md) appeared on Sympa 6.2.6.
