---
title: 'Sympa::Spindle::ProcessOutgoing(3)'
---

# NAME

Sympa::Spindle::ProcessOutgoing - Workflow of message distribution

# SYNOPSIS

    use Sympa::Spindle::ProcessOutgoing;

    my $spindle = Sympa::Spindle::ProcessOutgoing->new;
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessOutgoing](./Sympa-Spindle-ProcessOutgoing.3.md) defines workflow to distribute messages
in outgoing spool using mailer.

If messages are stored into incoming spool, sooner or later
[Sympa::Spindle::ProcessIncoming](./Sympa-Spindle-ProcessIncoming.3.md) fetches them, modifys header and body of
them, shelves several transformations, and at last stores altered messages
into outgoing spool.

When spin() method of this class is invoked, it reads the messages in outgoing
spool and executes shelved transformations.
Message transformations are done in the following order:

- DMARC protection
- Processing for tracking and VERP (see also <Sympa::Tracking>)
- Personalization (a.k.a. "merge")
- S/MIME signing
- S/MIME encryption
- Removal of existing DKIM signature(s) which are invalidated by
preceding transformations.
- DKIM signing

Then spin() method stores transformed message into mailer
(See [Sympa::Mailer](./Sympa-Mailer.3.md)).

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

- new ( \[ log\_level => $level \], \[ log\_smtp => 0|1 \] )
- spin ( )

    new() may take following options:

    - log\_level => $level

        Overwrites log\_level parameter in configuration.

    - log\_smtp => 0|1

        Overwrites log\_smtp parameter in configuration.

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa-Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Bulk](./Sympa-Bulk.3.md) class.

# SEE ALSO

[Sympa::Bulk](./Sympa-Bulk.3.md), [Sympa::Mailer](./Sympa-Mailer.3.md), [Sympa::Message](./Sympa-Message.3.md), [Sympa::Spindle](./Sympa-Spindle.3.md).

# HISTORY

[Sympa::Spindle::ProcessOutgoing](./Sympa-Spindle-ProcessOutgoing.3.md) appeared on Sympa 6.2.13.
