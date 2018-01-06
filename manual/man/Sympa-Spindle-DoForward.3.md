---
title: 'Sympa::Spindle::DoForward(3)'
---

# NAME

Sympa::Spindle::DoForward - Workflow to forward messages to administrators

# DESCRIPTION

[Sympa::Spindle::DoForward](./Sympa-Spindle-DoForward.3.md) handles a message sent to \[list\]-editor (the list
editor), \[list\]-request (the list owner) or the listmaster.

If a message has one of types above, message will be forwarded to the users
according to types using mailer directly (See [Sympa::Mailer](./Sympa-Mailer.3.md)).
Otherwise messages will be skipped.

## Public methods

See also ["Public methods" in Sympa::Spindle::Incoming](./Sympa-Spindle-Incoming.3.md#public-methods).

- new ( key => value, ... )
- spin ( )

    In most cases, [Sympa::Spindle::ProcessIncoming](./Sympa-Spindle-ProcessIncoming.3.md) splices meessages
    to this class.  These methods are not used in ordinal case.

# SEE ALSO

[Sympa::Mailer](./Sympa-Mailer.3.md), [Sympa::Message](./Sympa-Message.3.md), [Sympa::Spindle::ProcessIncoming](./Sympa-Spindle-ProcessIncoming.3.md).

# HISTORY

[Sympa::Spindle::DoForward](./Sympa-Spindle-DoForward.3.md) appeared on Sympa 6.2.13.
