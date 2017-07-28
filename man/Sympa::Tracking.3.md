# NAME

Sympa::Tracking - Spool for message tracking

# SYNOPSIS

TBD.

# DESCRIPTION

The tracking feature is a way to request Delivery Status Notification (DSN) or
DSN and Message Disposition Notification (MDN) when sending a 
message to each subscribers. In that case, Sympa (bounced.pl) collect both 
DSN and MDN and store them in tracking spools.
Thus, for each message, the user can know which subscribers has displayed,
received or not received the message. This can be used for some important 
list where list owner need to collect the proof of reception or display of 
each message.

## Methods

- new ( context => $list )

    _Constructor_.
    Creates new [Sympa::Tracking](./Sympa::Tracking.3.md) instance.

    Parameter:

    - context => $list

        [Sympa::List](./Sympa::List.3.md) object.

    Returns:

    New [Sympa::Tracking](./Sympa::Tracking.3.md) object or `undef`.
    If unrecoverable error occurred, this method will die.

- db\_fetch ( recipient => $email, envid => $envid )

    TBD.

- get\_recipients\_status

    TBD.

- register ( $message, $rcpts, reception\_option => $mode )

    _Instance method_.
    Initializes notification table for each subscriber.

    Parameters:

    - $message

        The message.

    - $rcpts

        An arrayref of recipients.

    - reception\_option => $mode

        The reception option of those subscribers.

    Returns:

    `1` or `undef`.

- store ( $message, $rcpt,
\[ envid => $envid, status => $status, type => $type,
arrival\_date => $datestring \] )

    _Instance method_.
    Store notification into tracking spool.

    Parameters:

    - $message

        Notification message.

    - $rcpt

        E-mail address of recipient of original message.

    - envid => $envid, status => $status, type => $type,
    arrival\_date => $datestring

        If these optional parameters are specified,
        notification table is updated.

    Returns:

    True value if storing succeed.  Otherwise false.

- find\_notification\_id\_by\_message

    TBD.

- remove\_message\_by\_email

    TBD.
    Introduced on Sympa 6.2.19b.

- remove\_message\_by\_id

    TBD.

- remove\_message\_by\_period

    TBD.

# SEE ALSO

bounced(8), [Sympa::Message](./Sympa::Message.3.md), [Sympa::Spool::Bounce](./Sympa::Spool::Bounce.3.md).

# HISTORY

The tracking feature was contributed by
Guillaume Colotte and laurent Cailleux,
French army DGA Information Superiority.

[Sympa::Tracking](./Sympa::Tracking.3.md) module appeared on Sympa 6.2.
