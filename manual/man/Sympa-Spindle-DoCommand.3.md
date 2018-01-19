---
title: 'Sympa::Spindle::DoCommand(3)'
---

# NAME

Sympa::Spindle::DoCommand - Workflow to handle command messages

# DESCRIPTION

[Sympa::Spindle::DoCommand](./Sympa-Spindle-DoCommand.3.md) handles command messages bound for sympa,
\[list\]-subscribe or \[list\]-unsubscribe address.

If a message has one of types above, commands in the message will be parsed
and executed.  Otherwise messages will be skipped.

## Public methods

See also ["Public methods" in Sympa::Spindle::ProcessIncoming](./Sympa-Spindle-ProcessIncoming.3.md#public-methods).

- new ( key => value, ... )
- spin ( )

    In most cases, [Sympa::Spindle::ProcessIncoming](./Sympa-Spindle-ProcessIncoming.3.md) splices messages
    to this class.  These methods are not used in ordinal case.

# SEE ALSO

[Sympa::Message](./Sympa-Message.3.md), [Sympa::Spindle::ProcessIncoming](./Sympa-Spindle-ProcessIncoming.3.md),
[Sympa::Spindle::ProcessMessage](./Sympa-Spindle-ProcessMessage.3.md).

# HISTORY

[Sympa::Spindle::DoCommand](./Sympa-Spindle-DoCommand.3.md) appeared on Sympa 6.2.13.
