# NAME

Sympa::Spindle::ProcessAutomatic - Workflow of automatic list creation

# SYNOPSIS

    use Sympa::Spindle::ProcessAutomatic;

    my $spindle = Sympa::Spindle::ProcessAutomatic->new;
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessAutomatic](./Sympa-Spindle-ProcessAutomatic.3.md) defines workflow to process messages
for automatic list creation.

When spin() method is invoked, it reads the messages in automatic spool.
If the list a message is bound for has not been there and list creation is
authorized, it will be created.  Then the message is stored into incoming
message spool again and waits for processing by
[Sympa::Spindle::ProcessIncoming](./Sympa-Spindle-ProcessIncoming.3.md).

Order to process messages in source spool are controlled by modification time
of files and delivery date.
Some messages are skipped according to these priorities
(See [Sympa::Spool::Automatic](./Sympa-Spool-Automatic.3.md)):

- Messages with lowest priority (`z` or `Z`) are skipped.
- Messages with possiblly higher priority are chosen.
This is done by skipping messages with lower priority than those already
found.

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

- new ( \[ keepcopy => $directory \],
\[ log\_level => $level \],
\[ log\_smtp => 0|1 \] )
- spin ( )

    new() may take following options:

    - keepcopy => $directory

        spin() keeps copy of successfully processed messages in $directory.

    - log\_level => $level

        Overwrites log\_level parameter in configuration.

    - log\_smtp => 0|1

        Overwrites log\_smtp parameter in configuration.

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa-Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Spool::Automatic](./Sympa-Spool-Automatic.3.md) class.

# SEE ALSO

[Sympa::Message](./Sympa-Message.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spool::Automatic](./Sympa-Spool-Automatic.3.md), [Sympa::Spool::Incoming](./Sympa-Spool-Incoming.3.md).

# HISTORY

[Sympa::Spindle::ProcessAutomatic](./Sympa-Spindle-ProcessAutomatic.3.md) appeared on Sympa 6.2.10.
