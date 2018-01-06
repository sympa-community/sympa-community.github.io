---
title: 'Sympa::Spindle::TransformOutgoing(3)'
---

# NAME

Sympa::Spindle::TransformOutgoing -
Process to transform messages - second stage

# DESCRIPTION

This class executes the second stage of message transformation to be sent
through the list. This stage is put after storing messages into archive
spool (See also [Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md)).
Transformation processes by this class are done in the following order:

- Executes `post_archive` hook of [message hooks](./Sympa-Message-Plugin.3.md)
if available.
- Adds / modifies `Reply-To` header field,
if [`reply_to_header`](./list_config.5.md#reply_to_header) list option is
enabled.
- Adds / overwrites following header fields:
    - `X-Loop`
    - `X-Sequence`
    - `Errors-To`
    - `Precedence`
    - `Sender`
    - `X-no-archive`
- Adds header fields specified by
[`custom_header`](./list_config.5.md#custom_header) list configuration parameter,
if any.
- Adds RFC 2919 `List-Id` field,
RFC 2369 fields (according to
[`rfc2369_header_fields`](./list_config.5.md#rfc2369_header_fields) list
configuration option) and RFC 5064 `Archived-At` field (if archiving is
enabled).
- Removes header fields specified by
[`remove_outgoing_headers`](./list_config.5.md#remove_outgoing_headers)
list configuration parameter, if any.

Then this class passes the message to the last stage of transformation,
[Sympa::Spindle::ToList](./Sympa-Spindle-ToList.3.md).

# CAVEAT

- Transformation by this class can break integrity of DKIM signature,
because some header fields may be removed according to
`remove_outgoing_headers` list configuration parameter.

# SEE ALSO

[Sympa::Internals::Workflow](./Sympa-Internals-Workflow.3.md).

[Sympa::Message](./Sympa-Message.3.md),
[Sympa::Message::Plugin](./Sympa-Message-Plugin.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md),
[Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md).

# HISTORY

[Sympa::Spindle::TransformOutgoing](./Sympa-Spindle-TransformOutgoing.3.md) appeared on Sympa 6.2.13.
