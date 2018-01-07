---
title: 'Sympa::Spindle::DistributeMessage(3)'
---

# NAME

Sympa::Spindle::DistributeMessage -
Workflow to distribute messages to list members

# DESCRIPTION

[Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md) distributes incoming messages to list
members.

This class represents the series of following processes:

- [Sympa::Spindle::TransformIncoming](./Sympa-Spindle-TransformIncoming.3.md)

    Process to transform messages - first stage

- [Sympa::Spindle::ToArchive](./Sympa-Spindle-ToArchive.3.md)

    Process to store messages into archiving spool

- [Sympa::Spindle::TransformOutgoing](./Sympa-Spindle-TransformOutgoing.3.md)

    Process to transform messages - second stage

- [Sympa::Spindle::ToDigest](./Sympa-Spindle-ToDigest.3.md)

    Process to store messages into digest spool

- [Sympa::Spindle::ToList](./Sympa-Spindle-ToList.3.md)

    Process to distribute messages to list members

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

- new ( key => value, ... )

    In most cases, [Sympa::Spindle::DoMessage](./Sympa-Spindle-DoMessage.3.md)
    splices messages to this class.  This method is not used in ordinal case.

- spin ( )

    Not implemented.

# SEE ALSO

[Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spindle::DoMessage](./Sympa-Spindle-DoMessage.3.md),
[Sympa::Spindle::ProcessModeration](./Sympa-Spindle-ProcessModeration.3.md).

# HISTORY

[Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md) appeared on Sympa 6.2.13.
