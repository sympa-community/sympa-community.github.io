# NAME

Sympa::Spool::Auth - Spool for held requests waiting for moderation

# SYNOPSIS

    use Sympa::Spool::Auth;

    my $spool = Sympa::Spool::Auth->new;
    my $request = Sympa::Request->new(...);
    $spool->store($request);

    my $spool = Sympa::Spool::Auth->new(
        context => $list, action => 'add');
    my $size = $spool->size;

    my $spool = Sympa::Spool::Auth->new(
        context => $list, keyauth => $id, action => 'add');
    my ($request, $handle) = $spool->next;

    $spool->remove($handle);

# DESCRIPTION

[Sympa::Spool::Auth](./Sympa-Spool-Auth.3.md) implements the spool for held requests waiting
for moderation.

## Methods

See also ["Public methods" in Sympa::Spool](./Sympa-Spool.3.md#public-methods).

- new ( \[ context => $list \], \[ action => $action \],
\[ keyauth => $id \], \[ email => $email \])
- next ( \[ no\_lock => 1 \] )

    If the pairs describing metadatas are specified,
    contents returned by next() are filtered by them.

    Order of items returned by next() is controlled by time of submission.

- quarantine ( )

    Does nothing.

## Context and metadata

See also ["Marshaling and unmarshaling metadata" in Sympa::Spool](./Sympa-Spool.3.md#marshaling-and-unmarshaling-metadata).

This class particularly gives following metadata:

- {action}

    Action requested.
    `'add'` etc.

- {date}

    Unix time when the request was submitted.

- {email}

    E-mail of user who submitted the request, or target e-mail of the request.

- {keyauth}

    Authentication key generated automatically
    when the request is stored to spool.

# CONFIGURATION PARAMETERS

Following site configuration parameters in sympa.conf will be referred.

- queuesubscribe

    Directory path of held request spool.

    Note:
    Named such by historical reason.

# SEE ALSO

[sympa\_msg(8)](./sympa_msg.8.md), [wwsympa(8)](./wwsympa.8.md),
[Sympa::Request](./Sympa-Request.3.md), [Sympa::Spool](./Sympa-Spool.3.md).

# HISTORY

[Sympa::Spool::Request](./Sympa-Spool-Request.3.md) appeared on Sympa 6.2.10.
It was renamed to [Sympa::Spool::Auth](./Sympa-Spool-Auth.3.md) on Sympa 6.2.13.
