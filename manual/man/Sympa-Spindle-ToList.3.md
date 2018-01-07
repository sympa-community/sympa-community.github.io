---
title: 'Sympa::Spindle::ToList(3)'
---

# NAME

Sympa::Spindle::ToList - Process to distribute messages to list members

# DESCRIPTION

This class executes the last stage of message transformation to be sent
through the list.
Transformation processes by this class are done in the following order:

- Classifies recipients for whom message is delivered by each reception mode,
filters recipients by topics (see also [Sympa::Topic](./Sympa-Topic.3.md)), and choose
message tracking modes if necessary.
- Transforms message by each reception mode.
- Enables DMARC protection (according to
[`dmarc_protection`](./list_config.5.md#dmarc_protection)
list configuration parameter),
message personalization (according to
[`merge_feature`](./list_config.5.md#merge_feature)
list configuration parameter) and/or
re-encryption by S/MIME (if original message was encrypted).
- Alters envelope sender of the message to _list_`-owner` address.

Then stores message into outgoing spool (see [Sympa::Bulk](./Sympa-Bulk.3.md))
with classified packets of recipients.

This cass updates statistics information of the list (with digest delivery,
[Sympa::Spindle::ToOutgoing](./Sympa-Spindle-ToOutgoing.3.md) will update it).

# SEE ALSO

[Sympa::Internals::Workflow](./Sympa-Internals-Workflow.3.md).

[Sympa::Bulk](./Sympa-Bulk.3.md),
[Sympa::Message](./Sympa-Message.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md),
[Sympa::Topic](./Sympa-Topic.3.md), [Sympa::Tracking](./Sympa-Tracking.3.md).

# HISTORY

[Sympa::Spindle::ToList](./Sympa-Spindle-ToList.3.md) appeared on Sympa 6.2.13.
