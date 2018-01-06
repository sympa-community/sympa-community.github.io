---
title: 'Sympa::Spindle::ProcessHeld(3)'
---

# NAME

Sympa::Spindle::ProcessHeld - Workflow of message confirmation

# SYNOPSIS

    use Sympa::Spindle::ProcessHeld;

    my $spindle = Sympa::Spindle::ProcessHeld->new(
        confirmed_by => $email, context => $robot, authkey => $key);
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessHeld](./Sympa-Spindle-ProcessHeld.3.md) defines workflow for confirmation of held
messages.

When spin() method is invoked, it reads a message in held message spool,
authorizes it and distribute it if possible.
Either authorization and distribution failed or not, spin() will terminate
processing.
Failed message will be kept in spool and wait for confirmation again.

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

- new ( confirmed\_by => $email,
context => $context, authkey => $key,
\[ quiet => 1 \] )
- spin ( )

    new() must take following options:

    - confirmed\_by => $email

        E-mail address of the user who confirmed the message.
        It is given by CONFIRM command and
        used by [Sympa::Spindle::AuthorizeMessage](./Sympa-Spindle-AuthorizeMessage.3.md) to execute "send" scenario.

    - context => $context
    - authkey => $key

        Context (List or Robot) and authorization key to specify the message in
        spool.

    - quiet => 1

        If this option is set, automatic replies reporting result of processing
        to the user (see ["confirmed\_by"](#confirmed_by)) will not be sent.

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa-Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Spool::Held](./Sympa-Spool-Held.3.md) class.

- {finish}

    `'success'` is set if processing succeeded.
    `'failure'` is set if processing failed.

# SEE ALSO

[Sympa::Message](./Sympa-Message.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spindle::AuthorizeMessage](./Sympa-Spindle-AuthorizeMessage.3.md),
[Sympa::Spool::Held](./Sympa-Spool-Held.3.md).

# HISTORY

[Sympa::Spindle::ProcessHeld](./Sympa-Spindle-ProcessHeld.3.md) appeared on Sympa 6.2.13.
