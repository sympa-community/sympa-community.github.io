# NAME

Sympa::Spindle::AuthorizeMessage -
Workflow to authorize messages bound for lists

# DESCRIPTION

[Sympa::Spindle::AuthorizeMessage](./Sympa::Spindle::AuthorizeMessage.3.md) authorizes messages and stores them
into confirmation spool, moderation spool or the lists.

TBD

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa::Spindle.3.md#public-methods).

- new ( key => value, ... )

    In most cases, [Sympa::Spindle::DoMessage](./Sympa::Spindle::DoMessage.3.md)
    splices meessages to this class.  This method is not used in ordinal case.

- spin ( )

    Not implemented.

# SEE ALSO

[Sympa::Message](./Sympa::Message.3.md), [Sympa::Scenario](./Sympa::Scenario.3.md), [Sympa::Spindle::DistributeMessage](./Sympa::Spindle::DistributeMessage.3.md),
[Sympa::Spindle::DoMessage](./Sympa::Spindle::DoMessage.3.md), [Sympa::Spindle::ProcessHeld](./Sympa::Spindle::ProcessHeld.3.md),
[Sympa::Spindle::ToEditor](./Sympa::Spindle::ToEditor.3.md), [Sympa::Spindle::ToHeld](./Sympa::Spindle::ToHeld.3.md),
[Sympa::Spindle::ToModeration](./Sympa::Spindle::ToModeration.3.md),
[Sympa::Topic](./Sympa::Topic.3.md).

# HISTORY

[Sympa::Spindle::AuthorizeMessage](./Sympa::Spindle::AuthorizeMessage.3.md) appeared on Sympa 6.2.13.
