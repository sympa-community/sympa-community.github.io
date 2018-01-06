---
title: 'Sympa::Spindle::ProcessIncoming(3)'
---

# NAME

Sympa::Spindle::ProcessIncoming - Workflow of processing incoming messages

# SYNOPSIS

    use Sympa::Spindle::ProcessIncoming;

    my $spindle = Sympa::Spindle::ProcessIncoming->new;
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessIncoming](./Sympa-Spindle-ProcessIncoming.3.md) defines workflow to process incoming
messages.

When spin() method is invoked, it reads the messages in incoming spool and
rejects, quarantines or modifyes them.
Processing are done in the following order:

- Checks if message has message ID and sender, and if not, quarantines it.
Because such messages will be source of various troubles.
- Checks if robot which message is bound for exists, and if not, rejects it.
- Checks spam status, DKIM signature and S/MIME signature,
and decrypts message if possible.
Result of these checks are stored in message object and used in succeeding
process.
- If message is bound for the list, checks if the list exists, and if not,
rejects it.
- Loop prevention.  If loop is detected, ignores message.
- Virus checking, if enabled by configuration.
And if malware is detected, rejects or discards message.
- Splices message to appropriate class according to the type of message:
[Sympa::Spindle::DoCommand](./Sympa-Spindle-DoCommand.3.md) for command message;
[Sympa::Spindle::DoForward](./Sympa-Spindle-DoForward.3.md) for message bound for administrator;
[Sympa::Spindle::DoMessage](./Sympa-Spindle-DoMessage.3.md) for ordinal post.

Order to process messages in source spool are controlled by modification time
of files and delivery date.
Some messages are skipped according to these priorities
(See [Sympa::Spool::Incoming](./Sympa-Spool-Incoming.3.md)):

- Messages with lowest priority (`z` or `Z`) are skipped.
- Messages with possiblly higher priority are chosen.
This is done by skipping messages with lower priority than those already
found.

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

- new ( \[ keepcopy => $directory \], \[ lang => $lang \],
\[ log\_level => $level \],
\[ log\_smtp => 0|1 \] )
- spin ( )

    new() may take following options:

    - keepcopy => $directory

        spin() keeps copy of successfully processed messages in $directory.

    - lang => $lang

        Overwrites lang parameter in configuration.

    - log\_level => $level

        Overwrites log\_level parameter in configuration.

    - log\_smtp => 0|1

        Overwrites log\_smtp parameter in configuration.

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa-Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Spool::Incoming](./Sympa-Spool-Incoming.3.md) class.

# SEE ALSO

[Sympa::Message](./Sympa-Message.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spindle::DoCommand](./Sympa-Spindle-DoCommand.3.md), [Sympa::Spindle::DoForward](./Sympa-Spindle-DoForward.3.md),
[Sympa::Spindle::DoMessage](./Sympa-Spindle-DoMessage.3.md),
[Sympa::Spool::Incoming](./Sympa-Spool-Incoming.3.md).

# HISTORY

[Sympa::Spindle::ProcessIncoming](./Sympa-Spindle-ProcessIncoming.3.md) appeared on Sympa 6.2.13.
