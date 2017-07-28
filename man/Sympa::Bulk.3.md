# NAME

Sympa::Bulk - Spool for bulk sending

# SYNOPSIS

    use Sympa::Bulk;
    my $bulk = Sympa::Bulk->new;

    $bulk->store($message, ['user@dom.ain', 'user@other.dom.ain']);

    my ($message, $handle) = $bulk->next;

# DESCRIPTION

[Sympa::Bulk](./Sympa::Bulk.3.md) implements the spool for bulk sending.

## Methods

- new ( )

    _Constructor_.
    Creates new instance of [Sympa::Bulk](./Sympa::Bulk.3.md).

- next ( )

    _Instance method_.
    Gets next packet to process, order is controlled by message priority, then by
    packet priority, then by delivery date, then by reception date.
    Packets with future delivery date are ignored.
    Packet will be locked to prevent multiple proccessing of a single packet.

    Parameters:

    None.

    Returns:

    Two-elements list of [Sympa::Message](./Sympa::Message.3.md) instance and filehandle locking
    a packet.

- quarantine ( $handle )

    _Instance method_.
    Quarantines a packet.
    Packet will be moved into bad/ subdirectory of the spool.

    Parameter:

    - $handle

        Filehandle, [Sympa::LockedFile](./Sympa::LockedFile.3.md) instance, locking packet.

    Returns:

    True value if packet could be quarantined.
    Otherwise false value.

- remove ( $handle )

    _Instance method_.
    Removes a packet.
    If the packet is the last one of bulk sending,
    corresponding message will also be removed from spool.

    Parameter:

    - $handle

        Filehandle, [Sympa::LockedFile](./Sympa::LockedFile.3.md) instance, locking packet.

    Returns:

    True value if packet could be removed.
    Otherwise false value.

- store ( $message, $rcpt, \[ original => $original \],
\[ tag => $tag \] )

    _Instance method_.
    Stores the message into message spool.
    Recipients will be split into multiple packets and
    stored into packet spool.

    Parameters:

    - $message

        Message to be stored.  Following attributes and metadata are referred:

        - {envelope\_sender}

            SMTP "MAIL FROM:" field.

        - {priority}

            Message priority.

        - {packet\_priority}

            Packet priority, assigned as `sympa_packet_priority` parameter by each robot.

        - {date}

            Unix time when the message would be delivered.

        - {time}

            Unix time in floating point number when the message was stored.

    - $rcpt

        Scalar, scalarref or arrayref, for SMTP "RCPT TO:" field(s).

    - original => $original

        If the message was decrypted, stores original encrypted form.

    - tag => $tag

        TBD.

    Returns:

    If storing succeeded, marshalled metadata (file name) of the message.
    Otherwise `undef`.

- too\_much\_remaining\_packets ( )

    _Instance method_.
    Returns true value if the number of remaining packets exceeds
    the value of the `bulk_fork_threshold` config parameter.

# SEE ALSO

[bulk(8)](./bulk.8.md), [Sympa::Mailer](./Sympa::Mailer.3.md), [Sympa::Message](./Sympa::Message.3.md).

# HISTORY

[Bulk](https://metacpan.org/pod/Bulk) module initially written by Serge Aumont appeared on Sympa 6.0.
It used database tables to store and fetch packets and messages.

Support for DKIM signing was added on Sympa 6.1.

Rewritten [Sympa::Bulk](./Sympa::Bulk.3.md) appeared on Sympa 6.2, using spools based on
filesystem.
