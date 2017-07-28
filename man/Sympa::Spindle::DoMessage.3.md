# NAME

Sympa::Spindle::DoMessage - Workflow to handle messages bound for lists

# DESCRIPTION

[Sympa::Spindle::DoMessage](./Sympa::Spindle::DoMessage.3.md) handles a message sent to a list.

If a message has no special types (command or administrator),
message will be processed.  Otherwise messages will be skipped.

TBD

## Public methods

See also ["Public methods" in Sympa::Spindle::Incoming](./Sympa::Spindle::Incoming.3.md#public-methods).

- new ( key => value, ... )
- spin ( )

    In most cases, [Sympa::Spindle::ProcessIncoming](./Sympa::Spindle::ProcessIncoming.3.md) splices meessages
    to this class.  These methods are not used in ordinal case.

# SEE ALSO

[Sympa::Message](./Sympa::Message.3.md), [Sympa::Spindle::AuthorizeMessage](./Sympa::Spindle::AuthorizeMessage.3.md),
[Sympa::Spindle::ProcessIncoming](./Sympa::Spindle::ProcessIncoming.3.md).

# HISTORY

[Sympa::Spindle::DoMessage](./Sympa::Spindle::DoMessage.3.md) appeared on Sympa 6.2.13.
