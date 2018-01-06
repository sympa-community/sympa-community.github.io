---
title: 'Sympa::Spindle::ProcessDigest(3)'
---

# NAME

Sympa::Spindle::ProcessDigest - Workflow of digest sending

# SYNOPSIS

    use Sympa::Spindle::ProcessDigest;

    my $spindle = Sympa::Spindle::ProcessDigest->new;
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessDigest](./Sympa-Spindle-ProcessDigest.3.md) defines workflow to distribute digest
messages.

When spin() method is invoked, messages kept in digest spool of each list are
compiled into digest messages (MIME digest, plain text digest or summary) and
stored into outgoing spool.
Lists not reaching the time to distribute digest are omitted.

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

- new ( \[ send\_now => 1 \], \[ keep\_digest => 1 \] )
- spin ( )

    If `send_now` is set, spin() stores digests of all lists keeping unsent
    digests into outgoing spool, including the lists not reaching time to
    distribute.
    If `keep_digest` is set, won't remove compiled messages from digest spool.

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa-Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Spool::Digest::Collection](./Sympa-Spool-Digest-Collection.3.md) class.

# SEE ALSO

[Sympa::Spindle](./Sympa-Spindle.3.md),
[Sympa::Spool::Digest](./Sympa-Spool-Digest.3.md), [Sympa::Spool::Digest::Collection](./Sympa-Spool-Digest-Collection.3.md).

# HISTORY

[Sympa::Spindle::SendDigest](./Sympa-Spindle-SendDigest.3.md) appeared on Sympa 6.2.10.
It was renamed to [Sympa::Spindle::ProcessDigest](./Sympa-Spindle-ProcessDigest.3.md) on Sympa 6.2.13.
