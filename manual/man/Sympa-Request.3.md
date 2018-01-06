---
title: 'Sympa::Request(3)'
---

# NAME

Sympa::Request - Requests for operation

# SYNOPSYS

    use Sympa::Request;
    my $request = Sympa::Request->new($serialized, context => $list);
    my $request = Sympa::Request->new(context => $list, action => 'last');

# DESCRIPTION

[Sympa::Request](./Sympa-Request.3.md) inmplements serializable object representing requests by
users.

## Methods

- new ( \[ $serialized, \] context => $that, action => $action,
key => value, ... \] )

    _Constructor_.
    Creates a new [Sympa::Request](./Sympa-Request.3.md) object.

    Parameters:

    - $serialized

        Serialized request.

    - context => object

        Context.  [Sympa::List](./Sympa-List.3.md) object, Robot or `'*'`.

    - action => $action

        Name of requested action.

    - key => value, ...

        Metadata and attributes.

    Returns:

    A new instance of [Sympa::Request](./Sympa-Request.3.md), or _undef_, if something went wrong.

- new\_from\_tuples ( key => value, ... )

    _Constructor_.
    OBSOLETED.
    Creates [Sympa::Request](./Sympa-Request.3.md) object from paired options.

- dup ( )

    _Copy constructor_.
    Gets deep copy of instance.

- to\_string ( )

    _Serializer_.
    Returns serialized data of object.

- cmd\_line ( \[ canonic => 1 \] )

    _Instance method_.
    TBD.

- handler ( )

    _Instance method_.
    Name of a subclass of [Sympa::Request::Handler](./Sympa-Request-Handler.3.md) to process request.

- get\_id ( )

    _Instance method_.
    Gets unique identifier of instance.

## Context and metadata

Context and metadata given to constructor are accessible as hash elements
of object.
They are given by request spool.
See ["Context and metadata" in Sympa::Spool::Auth](./Sympa-Spool-Auth.3.md#context-and-metadata) for details.

## Attributes

These are accessible as hash elements of objects.
There are attributes including:

- {custom\_attribute}

    Custom attribute connected to requested action.

- {gecos}

    Display name of user sending request.

- {sender}

    E-mail of user who sent the request.

## Serialization

[Sympa::Request](./Sympa-Request.3.md) object includes number of slots as hash items:
**metadata**, **context** and **attributes**.
Metadata including context are given by spool:
See ["Marshaling and unmarshaling metadata" in Sympa::Spool](./Sympa-Spool.3.md#marshaling-and-unmarshaling-metadata).

Logically, objects are stored into physical spool as **serialized form**
and deserialized when they are fetched from spool.
Attributes are encoded in `X-Sympa-*:` pseudo-header fields.

See also ["Serialization" in Sympa::Message](./Sympa-Message.3.md#serialization) for example.

# SEE ALSO

[Sympa::Request::Collection](./Sympa-Request-Collection.3.md),
[Sympa::Request::Handler](./Sympa-Request-Handler.3.md),
[Sympa::Request::Message](./Sympa-Request-Message.3.md),
[Sympa::Spool::Auth](./Sympa-Spool-Auth.3.md).

# HISTORY

[Sympa::Request](./Sympa-Request.3.md) appeared on Sympa 6.2.10.
