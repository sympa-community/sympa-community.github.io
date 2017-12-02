# NAME

Sympa::Mailer - Store messages to sendmail

# SYNOPSIS

    use Sympa::Mailer;
    use Sympa::Process;
    my $mailer = Sympa::Mailer->instance;
    my $process = Sympa::Process->instance;

    $mailer->store($message, ['user1@dom.ain', user2@other.dom.ain']);

# DESCRIPTION

[Sympa::Mailer](./Sympa-Mailer.3.md) implements the class to invoke sendmail processes and
store messages to them.

## Methods

- instance ( )

    _Constructor_.
    Creates a singleton instance of [Sympa::Mailer](./Sympa-Mailer.3.md) object.

    Returns:

    A new [Sympa::Mailer](./Sympa-Mailer.3.md) instance, or _undef_ for failure.

- reaper ( \[ blocking => 1 \] )

    DEPRECATED.
    Use ["reap\_child" in Sympa::Process](./Sympa-Process.3.md#reap_child).

    _Instance method_.
    Non blocking function called by: main loop of sympa, task\_manager, bounced
    etc., just to clean the defuncts list by waiting to any processes and
    decrementing the counter.

    Parameter:

    - blocking => 1

        Operation would block.

    Returns:

    PID.

- store ( $message, $rcpt,
\[ envid => $envid \], \[ tag => $tag \] )

    _Instance method_.
    Makes a sendmail ready for the recipients given as argument, uses a file
    descriptor in the smtp table which can be imported by other parties.
    Before, waits for number of children process < number allowed by sympa.conf

    Parameters:

    - $message

        Message to be sent.

        {envelope\_sender} attribute of the message will be used as SMTP "MAIL FROM:"
        field.

    - $rcpt

        Scalar, scalarref or arrayref, for SMTP "RCPT TO:" field.

    - envid => $envid

        An envelope ID of this message submission in notification table.
        See also [Sympa::Tracking](./Sympa-Tracking.3.md).

    - tag => $tag

        TBD

    Returns:

    Filehandle on opened pipe to output SMTP "DATA" field.
    Otherwise `undef`.

## Attributes

[Sympa::Mailer](./Sympa-Mailer.3.md) instance may have following attributes:

- {log\_smtp}

    If true value is set, each invocation of sendmail process will be logged.

- {redundancy}

    Positive integer.
    If set, maximum number of invocation of sendmail is divided by this value.

# SEE ALSO

[Sympa::Alarm](./Sympa-Alarm.3.md), [Sympa::Bulk](./Sympa-Bulk.3.md), [Sympa::Message](./Sympa-Message.3.md), [Sympa::Process](./Sympa-Process.3.md).

# HISTORY

[Sympa::Mailer](./Sympa-Mailer.3.md), the rewrite of mail.pm, appeared on Sympa 6.2.
