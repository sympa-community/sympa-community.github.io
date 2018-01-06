---
title: 'Sympa::Request::Collection(3)'
---

# NAME

Sympa::Request::Collection - Collection of requests

# SYNOPSIS

    use Sympa::Request::Collection;
    $spool = Sympa::Request::Collection->new(
        context => $list, action => $action, sender => $sender,
        key1 => $val1, key2 => [$val2]);
    ($request, $handle) = $spool->next;

# DESCRIPTION

[Sympa::Request::Collection](./Sympa-Request-Collection.3.md) provides pseudo-spool to generate a set of
[Sympa::Request](./Sympa-Request.3.md) instances.

## Methods

- new ( context => $that, action => $action,
sender => $sender, \[ key => val, ... \] )
- next ( )

    next() returns [Sympa::Request](./Sympa-Request.3.md) instances.

    Options given to new() are used to generate instances.
    If one of their value is arrayref, next() repeatedly generates instances
    over each array item.

- quarantine ( )
- remove ( )
- store ( )

    Do nothing.

# SEE ALSO

[Sympa::Request](./Sympa-Request.3.md), [Sympa::Spool](./Sympa-Spool.3.md).

# HISTORY

[Sympa::Request::Collection](./Sympa-Request-Collection.3.md) appeared on Sympa 6.2.15.
