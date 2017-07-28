# NAME

Sympa::Spool::Moderation - Spool for held messages waiting for moderation

# SYNOPSIS

    use Sympa::Spool::Moderation;

    my $spool = Sympa::Spool::Moderation->new;
    my $modkey = $spool->store($message);

    my $spool =
        Sympa::Spool::Moderation->new(context => $list, authkey => $modkey);
    my ($message, $handle) = $spool->next;

    $spool->remove($handle, action => 'distribute');
    $spool->remove($handle);

# DESCRIPTION

[Sympa::Spool::Moderation](./Sympa-Spool-Moderation.3.md) implements the spool for held messages waiting
for moderation.

## Methods

See also ["Public methods" in Sympa::Spool](./Sympa-Spool.3.md#public-methods).

- new ( \[ context => $list \], \[ authkey => $modkey \] )
- next ( \[ no\_lock => 1 \] )

    If the pairs describing metadatas are specified,
    contents returned by next() are filtered by them.

- quarantine ( )

    Does nothing.

- remove ( $handle, \[ action => 'distribute' \] )

    If action is specified, rename message file to add it as extension, instead of
    removing message file.
    Otherwise, removes message file.

- size ( )

    Returns number of messages in the spool except which have extension.

- store ( $message, \[ original => $original \] )

    If storing succeeded, returns moderation key.

## Methods specific to this module

- html\_remove ( $metadata )

    _Instance method_.
    TBD.

    Parameters:

    - $metadata

        Hashref or message containing metadata.
        At least `context` and `authkey` are required.

    Returns:

    None.

- html\_store ( $message, $modkey )

    _Instance method_.
    Caches HTML view of message.

    Parameters:

    - $message

        Message to be stored.

    - $modkey

        Moderation key.

    Returns:

    None.

## Context and metadata

See also ["Marshaling and unmarshaling metadata" in Sympa::Spool](./Sympa-Spool.3.md#marshaling-and-unmarshaling-metadata).

This class particularly gives following metadata:

- {authkey}

    Moderation key generated automatically
    when the message is stored into spool.

- {validated}

    Keeps a string representing extension, if message has been renamed using
    remove() with option.

# CONFIGURATION PARAMETERS

Following site configuration parameters in sympa.conf will be referred.

- queuemod

    Directory path of moderation spool.

- viewmail\_dir

    Root directory path of directories where HTML view of messages are cached.

# SEE ALSO

[sympa\_msg(8)](./sympa_msg.8.md), [wwsympa(8)](./wwsympa.8.md),
[Sympa::Message](./Sympa-Message.3.md), [Sympa::Spool](./Sympa-Spool.3.md).

# HISTORY

[Sympa::Spool::Moderation](./Sympa-Spool-Moderation.3.md) appeared on Sympa 6.2.8.
