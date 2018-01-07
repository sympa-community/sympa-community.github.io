---
title: 'Sympa::Alarm(3)'
---

# NAME

Sympa::Alarm - Spool on memory for listmaster notification

# SYNOPSIS

    use Sympa::Alarm;
    my $alarm = Sympa::Alarm->instance;

    $alarm->store($message, $rcpt, $operation);

    $alarm->flush();
    $alarm->flush(purge => 1);

# DESCRIPTION

[Sympa::Alarm](./Sympa-Alarm.3.md) implements on-memory spool for listmaster notification.

## Methods

- instance ( )

    _Constructor_.
    Creates a singleton instance of [Sympa::Alarm](./Sympa-Alarm.3.md) object.

    Returns:

    A new [Sympa::Alarm](./Sympa-Alarm.3.md) instance, or undef for failure.

- store ( $message, $rcpt, operation => $operation )

    _Instance method_.
    Stores a message of a operation to spool.

    Parameters:

    - $message

        [Sympa::Message](./Sympa-Message.3.md) object to be stored.

    - $rcpt

        Arrayref or scalar.  Recipient of notification.

    - operation => $operation

        A string specifies tag of the message.

    Returns:

    True value if succeed, otherwise `undef`.

- flush ( \[ purge => $purge \] )

    _Instance method_.
    Sends compiled messages in spool.

    If true value is given as optional argument, all messages in spool will be
    sent.

## Attribute

The instance of [Sympa::Alarm](./Sympa-Alarm.3.md) has following attribute.

- {use\_bulk}

    If set to be true, messages to be sent will be stored into spool
    instead of being stored to sendmail.

    Default is false.

# HISTORY

Feature to compile notification to listmaster in group appeared on Sympa 6.2.

[Sympa::Alarm](./Sympa-Alarm.3.md) appeared on Sympa 6.2.
