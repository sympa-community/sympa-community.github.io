# NAME

Sympa::Spindle::ResendArchive - Workflow of resending messages in archive

# SYNOPSIS

    use Sympa::Spindle::ResendArchive;

    my $spindle = Sympa::Spindle::ResendArchive->new(
        resent_by => $email, context => $list, arc => $arc,
        message_id => $message_id);
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ResendArchive](./Sympa::Spindle::ResendArchive.3.md) defines workflow for resending of messages
in archive.

When spin() method is invoked, it reads a message in archive,
decorate and distribute it.
Either resending failed or not, spin() will terminate
processing.

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa::Spindle.3.md#public-methods).

- new ( resent\_by => $email,
context => $list, arc => $arc, message\_id => $message\_id,
\[ quiet => 1 \] )
- spin ( )

    new() must take following options:

    - resent\_by => $email

        E-mail address of the user who requested resending message.
        It is given by do\_send\_me() function of wwsympa.fcgi and
        used by [Sympa::Spindle::ToList](./Sympa::Spindle::ToList.3.md) to whom distribute message.

    - context => $list
    - arc => $arc
    - message\_id => $message\_id

        Context (List), archive and message ID to specify the message in archive.

    - quiet => 1

        NOT YET IMPLEMENTED.

        If this option is set, automatic replies reporting result of processing
        to the user (see ["resent\_by"](#resent_by)) will not be sent.

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa::Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Archive](./Sympa::Archive.3.md) class.

- {finish}

    `'success'` is set if processing succeeded.
    `'failure'` is set if processing failed.

# SEE ALSO

[Sympa::Archive](./Sympa::Archive.3.md),
[Sympa::Message](./Sympa::Message.3.md),
[Sympa::Spindle](./Sympa::Spindle.3.md), [Sympa::Spindle::ToList](./Sympa::Spindle::ToList.3.md),
[Sympa::Spool::TransformOutgoing](./Sympa::Spool::TransformOutgoing.3.md).

# HISTORY

[Sympa::Spindle::ResendArchive](./Sympa::Spindle::ResendArchive.3.md) appeared on Sympa 6.2.13.
