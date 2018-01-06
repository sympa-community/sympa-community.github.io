---
title: 'Sympa::Spindle::AuthorizeMessage(3)'
---

# NAME

Sympa::Spindle::AuthorizeMessage -
Workflow to authorize messages bound for lists

# DESCRIPTION

[Sympa::Spindle::AuthorizeMessage](./Sympa-Spindle-AuthorizeMessage.3.md) authorizes messages and stores them
into confirmation spool, moderation spool or the lists.

- Messages fetched from incoming (`msg`) spool or held (`auth`) spool may be
passed to this class (messages fetched from moderation spool won't be passed
to this class).
- Then this class checks the message with `send` scenario.
- According to the results of scenario processing, each message is passed
to any of classes for succeeding processing:
[Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md) for `do_it` (except if tagging topics
is required or when personalization failed);
[Sympa::Spindle::ToHeld](./Sympa-Spindle-ToHeld.3.md) for `request_auth` (except if personalization
failed);
[Sympa::Spindle::ToModeration](./Sympa-Spindle-ToModeration.3.md) for `editorkey` (except if personalization
failed);
[ympa::Spindle::ToEditor](https://metacpan.org/pod/ympa::Spindle::ToEditor) for `editor`;
otherwise reject it.

If the message was confirmed, i.e. it has been fetched from held spool and
at last decided to be distributed, `X-Validation-By` header field is added.
If the message at last will be distributed, `{shelved}` attribute (see
[Sympa::Message](./Sympa-Message.3.md)) is added as necessity.

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

- new ( key => value, ... )

    In most cases, [Sympa::Spindle::DoMessage](./Sympa-Spindle-DoMessage.3.md)
    splices meessages to this class.  This method is not used in ordinal case.

- spin ( )

    Not implemented.

# SEE ALSO

[Sympa::Internals::Workflow](./Sympa-Internals-Workflow.3.md).

[Sympa::Message](./Sympa-Message.3.md), [Sympa::Scenario](./Sympa-Scenario.3.md), [Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md),
[Sympa::Spindle::DoMessage](./Sympa-Spindle-DoMessage.3.md), [Sympa::Spindle::ProcessHeld](./Sympa-Spindle-ProcessHeld.3.md),
[Sympa::Spindle::ToEditor](./Sympa-Spindle-ToEditor.3.md), [Sympa::Spindle::ToHeld](./Sympa-Spindle-ToHeld.3.md),
[Sympa::Spindle::ToModeration](./Sympa-Spindle-ToModeration.3.md),
[Sympa::Topic](./Sympa-Topic.3.md).

# HISTORY

[Sympa::Spindle::AuthorizeMessage](./Sympa-Spindle-AuthorizeMessage.3.md) appeared on Sympa 6.2.13.
