# NAME

Sympa::Spindle::ProcessBounce - Workflow of bounce processing

# SYNOPSIS

    use Sympa::Spindle::ProcessBounce;

    my $spindle = Sympa::Spindle::ProcessBounce->new;
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessBounce](./Sympa::Spindle::ProcessBounce.3.md) defines workflow to process bounce messages
including notifications requested by tracking feature.

When spin() method is invoked, messages kept in bounce spool are
processed.
Bounce spool may contain several types of bounce messages by their
recipient addresses:

- Bounce address of particular list.
Messages bound for this address are analysed and increase bounce score
of original recipient if any.
- VERP address.
Messages bound for this address are stored into tracking spool
without envelope ID, and increase bounce score.
- VERP address with `w` or `r` suffix.
Messages bound for this address cause deletion of original recipient.
- VERP address with envelope ID.
Messages are Delivery Status Notification (DSN) or
Message Disposition Notification (MDN).
They are stored into tracking spool
with envelope ID, and increase bounce score.
- Others, and messages are E-mail Feedback Report.
Reports are analyzed, and if opt-out report is found and list configuration
allows it, original recipient will be deleted.

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa::Spindle.3.md#public-methods).

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa::Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Spool::Bounce](./Sympa::Spool::Bounce.3.md) class.

# SEE ALSO

[Sympa::Message](./Sympa::Message.3.md),
[Sympa::Spindle](./Sympa::Spindle.3.md), [Sympa::Spool::Bounce](./Sympa::Spool::Bounce.3.md), [Sympa::Tracking](./Sympa::Tracking.3.md).

# HISTORY

[Sympa::Spindle::ProcessBounce](./Sympa::Spindle::ProcessBounce.3.md) appeared on Sympa 6.2.10.
