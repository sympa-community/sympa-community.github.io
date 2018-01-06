---
title: 'Sympa::Spindle::ToArchive(3)'
---

# NAME

Sympa::Spindle::ToArchive - Process to store messages into archiving spool

# DESCRIPTION

This class stores message into archive spool (SPOOLDIR/outgoing).
However, in any of following cases, message won't be stored:

- `process_archive` list parameter is _not_ `on`, i.e. archiving is disabled.
- `ignore_x_no_archive_header_feature` list parameter is _not_ `on`,
and the message has any of these fields:

        X-no-archive: yes
        Restrict: no-external-archive

When message was originally encrypted,
then if `archive_crypted_msg` list parameter is _not_ `original`, decrypted
message will be stored.  Otherwise original message will be stored.

# SEE ALSO

[Sympa::Internals::Workflow](./Sympa-Internals-Workflow.3.md).

[Sympa::Message](./Sympa-Message.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md),
[Sympa::Spool::Archive](./Sympa-Spool-Archive.3.md).

# HISTORY

[Sympa::Spindle::ToArchive](./Sympa-Spindle-ToArchive.3.md) appeared on Sympa 6.2.13.
