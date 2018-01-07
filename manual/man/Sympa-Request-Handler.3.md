---
title: 'Sympa::Request::Handler(3)'
---

# NAME

Sympa::Request::Handler - Base class of request handler classes

# SYNOPSIS

    package Sympa::Request::Handler::foo;
    use base qw(Sympa::Request::Handler);
    
    use constant _action_regexp   => qr{reject|request_auth|do_it}i;
    use constant _action_scenario => 'review';
    use constant _context_class   => 'Sympa::List';
    
    sub _twist {
        ...
    }
    
    1;

# DESCRIPTION

[Sympa::Request::Handler](./Sympa-Request-Handler.3.md) is the base class of subclasses to process
instance of [Sympa::Request](./Sympa-Request.3.md).

## Methods

TBD.

## Methods subclass should implement

- \_action\_regexp ( )

    _Instance method_,
    _mandatory_ if \_action\_scenario() returns true value.
    Returns a regexp matching available scenario results.
    Note that `i` modifier is necessary.

- \_action\_scenario ( )

    _Instance method_,
    _mandatory_.
    Returns the name of scenario to authorize the request under given context.
    If authorization is not required, returns `undef`.

- \_context\_class ( )

    _Instance method_.
    Returns the class name of context under which the request will be executed,
    [Sympa::List](./Sympa-List.3.md) etc.
    By default, returns robot context.

- \_owner\_action ( )

    _Instance method_.
    Returns name of action to be stored in spool when scenario returns `owner`.
    By default, returns `undef`.

- \_twist ( $request )

    _Instance method_,
    _mandatory_.
    See ["\_twist" in Sympa::Spindle](./Sympa-Spindle.3.md#_twist).

# SEE ALSO

[Sympa::Request](./Sympa-Request.3.md), [Sympa::Spindle](./Sympa-Spindle.3.md).

# HISTORY

[Sympa::Request::Handler](./Sympa-Request-Handler.3.md) appeared on Sympa 6.2.15.
