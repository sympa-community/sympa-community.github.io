# NAME

Sympa::Spindle::ProcessModeration - Workflow of message moderation

# SYNOPSIS

    use Sympa::Spindle::ProcessModeration;

    my $spindle = Sympa::Spindle::ProcessModeration->new(
        distributed_by => $email, context => $robot, authkey => $key);
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessModeration](./Sympa-Spindle-ProcessModeration.3.md) defines workflow for moderation of
messages.

When spin() method is invoked, it reads a message in moderation spool and
distribute or reject it.
Either distribution or rejection failed or not, spin() will terminate
processing.
Failed message will be kept in spool and wait for moderation again.

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

- new ( distributed\_by => $email | rejected\_by => $email,
context => $context, authkey => $key,
\[ quiet => 1 \] )
- spin ( )

    new() must take following options:

    - distributed\_by => $email | rejected\_by => $email

        E-mail address of the user who distributed or rejected the message.
        It is given by DISTRIBUTE or REJECT command.

    - context => $context
    - authkey => $key

        Context (List or Robot) and authorization key to specify the message in
        spool.

    - quiet => 1

        If this option is set, automatic replies reporting result of processing
        to the user (see ["distributed\_by"](#distributed_by) and ["rejected\_by"](#rejected_by)) will not be sent.

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa-Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Spool::Moderation](./Sympa-Spool-Moderation.3.md) class.

- {finish}

    `'success'` is set if processing succeeded.
    `'failure'` is set if processing failed.

# SEE ALSO

[Sympa::Message](./Sympa-Message.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md),
[Sympa::Spool::Moderation](./Sympa-Spool-Moderation.3.md).

# HISTORY

[Sympa::Spindle::ProcessModeration](./Sympa-Spindle-ProcessModeration.3.md) appeared on Sympa 6.2.13.
