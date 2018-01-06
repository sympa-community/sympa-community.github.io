---
title: 'Sympa::Request::Message(3)'
---

# NAME

Sympa::Request::Message - Command message as spool of requests

# SYNOPSIS

    use Sympa::Request::Message;
    $spool = Sympa::Request::Message->new(message => $message);
    ($request, $handle) = $spool->next;

# DESCRIPTION

[Sympa::Request::Message](./Sympa-Request-Message.3.md) provides pseudo-spool to generate [Sympa::Request](./Sympa-Request.3.md)
instances from the [Sympa::Message](./Sympa-Message.3.md) instance.

## Methods

- new ( message => $message )
- next ( )

    Parses message $message and returns each command in it as [Sympa::Request](./Sympa-Request.3.md)
    instance.

# HISTORY

[Sympa::Request::Message](./Sympa-Request-Message.3.md) appeared in Sympa 6.2.13.
