---
title: 'Sympa::Spindle::ToOutgoing(3)'
---

# NAME

Sympa::Spindle::ToOutgoing - Process to store messages into outgoing spool

# DESCRIPTION

This class stores message into outgoing spool (SPOOLDIR/bulk).

If the message has list context and `add_list_statistics` attribute
(the case it was spliced from [Sympa::Spindle::ProcessDigest](./Sympa-Spindle-ProcessDigest.3.md)),
updates statistics information of the list (with regular delivery,
[Sympa::Spindle::ToList](./Sympa-Spindle-ToList.3.md) will update it).

# SEE ALSO

[Sympa::Message](./Sympa-Message.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md),
[Sympa::Bulk](./Sympa-Bulk.3.md).

# HISTORY

[Sympa::Spindle::ToOutgoing](./Sympa-Spindle-ToOutgoing.3.md) appeared on Sympa 6.2.13.
