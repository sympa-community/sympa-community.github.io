# NAME

Sympa::Spindle::ProcessArchive - Workflow of archive storage

# SYNOPSIS

    use Sympa::Spindle::ProcessArchive;

    my $spindle = Sympa::Spindle::ProcessArchive->new;
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessArchive](./Sympa-Spindle-ProcessArchive.3.md) defines workflow to store messages into
archves.

When spin() method is invoked, messages kept in archive spool are
processed.
Archive spool may contain two sorts of messages:
Normal messages and control messages.

- Normal messages have List context and may be stored into archive.
- Control messages have Robot context and their body contains one or more
command lines.  Following commands are available.
    - remove\_arc _listname_ _yyyy_-_mm_ _message-ID_

        Removes a message from archive _yyyy_-_mm_ of the list.
        Text message is preserved.

    - rebuildarc _listname_ \*

        Rebuilds all HTML archives of the list.

    - rebuildarc \* \*

        Rebuilds all HTML archives of all the lists on the robot.

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa-Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Spool::Archive](./Sympa-Spool-Archive.3.md) class.

# SEE ALSO

[Sympa::Archive](./Sympa-Archive.3.md), [Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spool::Archive](./Sympa-Spool-Archive.3.md).

# HISTORY

[Sympa::Spindle::StoreArchive](./Sympa-Spindle-StoreArchive.3.md) appeared on Sympa 6.2.10.
It was renamed to [Sympa::Spindle::ProcessArchive](./Sympa-Spindle-ProcessArchive.3.md) on Sympa 6.2.13.
