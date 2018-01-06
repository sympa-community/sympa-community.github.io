---
title: 'upgrade_bulk_spool(1)'
---

# NAME

upgrade\_bulk\_spool, upgrade\_bulk\_spool.pl - Migrating messages in bulk tables

# SYNOMSIS

    upgrade_bulk_spool.pl [ --dry_run ]

# DESCRIPTION

On Sympa earlier than 6.2, messages for bulk sending were stored into
bulk spool based on database tables.
Recent release of Sympa stores outbound messages into the spool based on
filesystem or sends them by Mailer directly.
This program migrates messages with old format in appropriate spool.

# OPTIONS

- --dry\_run

    Shows what will be done but won't really perform upgrade process.

# RETURN VALUE

This program exits with status 0 if processing secceeded.
Otherwise exits with non-zero status.

# CONFIGURATION OPTIONS

Following site configuration parameters in `/etc/sympa/sympa.conf` or
robot configuration parameters in `robot.conf` are referred.

- db\_type, db\_name etc.
- queuebulk
- sympa\_packet\_priority
- umask

# SEE ALSO

[sympa.conf(5)](./sympa.conf.5.md), [Sympa::Bulk](./Sympa-Bulk.3.md), [Sympa::Message](./Sympa-Message.3.md).

# HISTORY

upgrade\_bulk\_spool.pl appeared on Sympa 6.2.
