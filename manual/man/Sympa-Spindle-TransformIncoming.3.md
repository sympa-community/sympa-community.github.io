---
title: 'Sympa::Spindle::TransformIncoming(3)'
---

# NAME

Sympa::Spindle::TransformIncoming -
Process to transform messages - first stage

# DESCRIPTION

This class executes the first stage of message transformation to be sent
through the list. This stage is put before storing messages into archive
spool (See also [Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md)).
Transformation processes by this class are done in the following order:

- Executes `pre_distribute` hook of [message hooks](./Sympa-Message-Plugin.3.md)
if available.
- Adds `X-Sympa-Topic` header field, if any message topics
(see [Sympa::Topic](./Sympa-Topic.3.md)) are tagged for the message.
- Anonymizes message,
if [`anonymous_sender`](./list_config.5.md#anonymous_sender) list configuration
parameter is enabled.
- Adds custom subject tag to `Subject` field, if
[`custom_subject`](./list_config.5.md#custom_subject) list configuration
parameter is available.
- Enables message tracking (see [Sympa::Tracking](./Sympa-Tracking.3.md)) if necessary.
- Removes header fields specified by
[`remove_headers`](./list_config.5.md#remove_headers).

Then this class passes the message to the next stage of transformation.

# CAVEAT

- Transformation by this class can break integrity of DKIM signature,
because some header fields including `From`, `Message-ID` and `Subject` may
be altered.

# SEE ALSO

[Sympa::Internals::Workflow](./Sympa-Internals-Workflow.3.md).

[Sympa::Message](./Sympa-Message.3.md),
[Sympa::Message::Plugin](./Sympa-Message-Plugin.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md),
[Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md),
[Sympa::Topic](./Sympa-Topic.3.md),
[Sympa::Tracking](./Sympa-Tracking.3.md).

# HISTORY

[Sympa::Spindle::TransformIncoming](./Sympa-Spindle-TransformIncoming.3.md) appeared on Sympa 6.2.13.
