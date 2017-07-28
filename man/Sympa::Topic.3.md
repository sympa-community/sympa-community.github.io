# NAME

Sympa::Topic - Message topic

# SYNOPSIS

    use Sympa::Topic;
    
    $topic = Sympa::Topic->new(topic => $topics, method => 'auto');
    $topic->store($message);
    
    $topic = Sympa::Topic->load($message);

# DESCRIPTION

TBD.

## Methods

- new ( options... )

    _Constructor_.
    Creates new instance of [Sympa::Topic](./Sympa::Topic.3.md).

- load ( $message, \[ in\_reply\_to => 1 \] )

    _Constructor_.
    Looks for a msg topic file from the message\_id of
    the message, loads it and return contained information
    as hash items.

    Parameters:

    - $message

        [Sympa::Message](./Sympa::Message.3.md) instance to be looked for.

    - in\_reply\_to => 1

        Use value of `In-Reply-To:` field instead of message ID.

    Returns:

    Instance of [Sympa::Topic](./Sympa::Topic.3.md) or, if topic was not found, `undef`.

- store ( $message )

    _Instance method_.
    Tag the message by creating the msg topic file.

    Parameter:

    - $message

        Message to be tagged.

    Returns:

    Message topic filename or `undef`.

# CONFIGURATION PARAMETERS

- queuetopic

    Directory path where topic files are stored.

    Note:
    Though it is neither queue nor spool, named such by historical reason.

- umask

    The umask to make directory.

# HISTORY

Feature to handle message topics was introduced on Sympa 5.2b.

[Sympa::Topic](./Sympa::Topic.3.md) module appeared on Sympa 6.2.10.
