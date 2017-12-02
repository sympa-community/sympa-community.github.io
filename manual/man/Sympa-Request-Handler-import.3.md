# NAME

Sympa::Request::Handler::import - import request handler

# DESCRIPTION

Add subscribers to the list.
E-mails and display names of subscribers are taken from {dump} parameter,
the text including lines describing users to be added.

## Attributes

- {dump}

    _Mandatory_.
    Text including information of users to be added.

- {force}

    _Optional_.
    If true value is specified,
    users will be added even if the list is closed.

# SEE ALSO

[Sympa::Request::Handler](./Sympa-Request-Handler.3.md), [Sympa::Request::Handler::add](./Sympa-Request-Handler-add.3.md).

# HISTORY

[Sympa::Request::Handler::import](./Sympa-Request-Handler-import.3.md) appeared on Sympa 6.2.19b.
