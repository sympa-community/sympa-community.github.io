---
title: 'Sympa::Spindle::ToDigest(3)'
---

# NAME

Sympa::Spindle::ToDigest - Process to store messages into digest spool

# DESCRIPTION

If the list is configured to perform digest delivery, this class stores it
into digest spool (`SPOOLDIR/digest/list@domain`).

However, ecrypted messages will be ignored.

# SEE ALSO

[Sympa::Internals::Workflow](./Sympa-Internals-Workflow.3.md).

[Sympa::Message](./Sympa-Message.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md),
[Sympa::Spool::Digest](./Sympa-Spool-Digest.3.md).

# HISTORY

[Sympa::Spindle::ToDigest](./Sympa-Spindle-ToDigest.3.md) appeared on Sympa 6.2.13.
