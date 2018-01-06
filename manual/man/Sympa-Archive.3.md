---
title: 'Sympa::Archive(3)'
---

# NAME

Sympa::Archive - Archives of Sympa

# SYNOPSIS

    use Sympa::Archive;
    $archive = Sympa::Archive->new(context => $list);

    @arcs = $archive->get_archives;

    $archive->store($message);
    $archive->html_store($message);

    $archive->select_archive('2015-04');
    ($message, $handle) = $archive->next;

    $archive->select_archive('2015-04');
    ($message, $handle) = $archive->fetch(message_id => $message_id);
    $archive->html_remove($message_id);
    $archive->remove($handle);

    $archive->html_rebuild('2015-04');

# DESCRIPTION

[Sympa::Archive](./Sympa-Archive.3.md) implements the interface to handle archives.

## Methods and functions

- new ( context => $list )

    _Constructor_.
    Creates new instance of [Sympa::Archive](./Sympa-Archive.3.md).

    Parameter:

    - context => $list

        Context of object, a [Sympa::List](./Sympa-List.3.md) instance.

- add\_archive ( $arc )

    _Instance method_.
    Adds archive directory named $arc.
    Currently, archive directory must have the form `YYYY-MM`.

- purge\_archive ( $arc )

    _Instance method_.
    Removes archive directory and its content entirely.
    removed content can not be recovered.

- select\_archive ( $arc, \[ info => 1 \] )

    _Instance method_.
    Selects an archive directory.
    It will be referred by consequent operations.

- fetch ( message\_id => $message\_id )

    _Instance method_.
    Gets a message from archive.
    select\_archive() must be called in advance.

    Message will be locked to prevent multiple proccessing of a single message.

    Parameter:

    - message\_id => $message\_id

        Message ID of the message to be feteched.

    Returns:

    Two-elements list of [Sympa::Message](./Sympa-Message.3.md) instance and filehandle locking
    a message.

- html\_fetch ( file => $filename )

    _Instance method_.
    Gets a metadata of formatted message from HTML archive.
    select\_archive() must be called in advance.

    Parameter:

    - file => $filename

        File name of the message to be feteched.

    Returns:

    Hashref.
    Note that message won't be locked.

- get\_size ( )

    _Instance method_.
    Gets total size of messages in archives.
    This method was introduced on Sympa 6.2.17.

- next ( \[ reverse => 1 \] )

    _Instance method_.
    Gets next message in archive.
    select\_archive() must be called in advance.

    Message will be locked to prevent multiple proccessing of a single message.

    Parameters:

    None.

    Returns:

    Two-elements list of [Sympa::Message](./Sympa-Message.3.md) instance and filehandle locking
    a message.

- html\_next ( \[ reverse => 1 \] )

    _Instance method_.
    Gets next metadata of formatted message in archive.
    select\_archive() must be called in advance.

    Parameters:

    None.

    Returns:

    Hashref.
    Note that message will not be locked.

- remove ( $handle )

    _Instance method_.
    Removes a message from archive.

    Parameter:

    - $handle

        Filehandle, [Sympa::LockedFile](./Sympa-LockedFile.3.md) instance, locking message.
        It is returned by ["fetch"](#fetch)() or ["next"](#next)().

    Returns:

    True value if message could be removed.
    Otherwise false value.

- html\_remove ( $message\_id )

    _Instance method_.
    TBD.

- store ( $message )

    _Instance method_.
    Stores the message into archive.

    Parameters:

    - $message

        A [Sympa::Message](./Sympa-Message.3.md) instance to be stored.
        Following attributes and metadata are referred:

        - {date}

            Unix time when the message would be delivered.

    Returns:

    If storing succeeded, marshalled metadata (file name) of the message.
    Otherwise `undef`.

- html\_store ( $message )

    _Instance method_.
    TBD.

- get\_archives ( )

    _Instance method_.
    Gets a list of archive directories this archive contains.
    Items of returned value may be fed to select\_archive() and so on.

- html\_rebuild ( $arc )

    _Instance method_.
    Rebuilds archives for the list the name of which is given in the argument
    $arc.

    Parameters:

    - $arc

        A character string containing the name of archive directory in the list
        which we want to rebuild.

    Returns:

    _undef_ if something goes wrong.

- html\_format ( $message,
destination\_dir => $destination\_dir,
attachment\_url => $attachment\_url )

    _Function_.
    Converts a message to HTML.

    Parameters:

    - $message

        Message to be formatted.
        [Sympa::Message](./Sympa-Message.3.md) instance.

    - $destination\_dir

        The directory result is stored in.

    - $attachment\_url

        Base URL used to link attachments.

        Note:
        On 6.2.13 and earlier, this option was named "`attach**e**ment_url`".

        Note:
        On 6.2.17 and later, this option may take an arrayref value.
        In such case items will be percent-encoded and conjuncted.
        Otherwise if a string is given, it will not be encoded.

- get\_id ( )

    _Instance method_.
    Gets unique identifier of instance.

# SEE ALSO

[archived(8)](./archived.8.md), [mhonarc(1)](./mhonarc.1.md), [wwsympa(8)](./wwsympa.8.md), [Sympa::Message](./Sympa-Message.3.md).

# HISTORY

[Archive](https://metacpan.org/pod/Archive) was renamed to [Sympa::Archive](./Sympa-Archive.3.md) on Sympa 6.2.
