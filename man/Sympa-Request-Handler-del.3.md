# NAME

Sympa::Request::Handler::del - del request handler

# DESCRIPTION

Removes a user from a list (requested by another user).
Verifies the authorization and sends acknowledgements
unless quiet is specified.

## Attributes

See also ["Attributes" in Sympa::Request::Handler](./Sympa-Request-Handler.3.md#attributes).

- {email}

    _Mandatory_.
    E-mail of the user to be deleted.

- {force}

    _Optional_.
    If true value is specified,
    users will be deleted even if the list is closed.

- {quiet}

    _Optional_.
    Don't notify addition to the user.

# SEE ALSO

[Sympa::Request::Handler](./Sympa-Request-Handler.3.md).

# HISTORY
