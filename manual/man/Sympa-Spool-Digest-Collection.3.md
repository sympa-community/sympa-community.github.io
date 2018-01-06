---
title: 'Sympa::Spool::Digest::Collection(3)'
---

# NAME

Sympa::Spool::Digest::Collection - Collection of digest spools

# SYNOPSIS

    use Sympa::Spool::Digest::Collection;
    
    my $collection = Sympa::Spool::Digest::Collection->new;
    my ($spool, $handle) = $collection->next;

# DESCRIPTION

[Sympa::Spool::Digest::Collection](./Sympa-Spool-Digest-Collection.3.md) implements the collection of
[Sympa::Spool::Digest](./Sympa-Spool-Digest.3.md) instances.

## Methods

See also ["Public methods" in Sympa::Spool](./Sympa-Spool.3.md#public-methods).

- next ( )

    Returns next instance of [Sympa::Spool::Digest](./Sympa-Spool-Digest.3.md).
    Order is controlled by modification times of spool directories.
    Spool directory is locked to prevent processing by multiple processes.

- quarantine ( )

    Does nothing.

- remove ( $handle )

    Trys to remove directory of spool.
    If succeeded, returns true value.
    Otherwise returns false value.

- store (  )

    Does nothing.

# CONFIGURATION PARAMETERS

Following site configuration parameters in sympa.conf will be referred.

- queuedigest

    Parent directory path of digest spools.

# SEE ALSO

[sympa\_msg(8)](./sympa_msg.8.md), [Sympa::Spool::Digest](./Sympa-Spool-Digest.3.md).

# HISTORY

[Sympa::Spool::Digest::Collection](./Sympa-Spool-Digest-Collection.3.md) appeared on Sympa 6.2.6.
