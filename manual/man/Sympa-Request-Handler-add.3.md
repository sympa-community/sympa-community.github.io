---
title: 'Sympa::Request::Handler::add(3)'
---

# NAME

Sympa::Request::Handler::add - add request handler

# DESCRIPTION

Adds a user to a list (requested by another user). Verifies
the proper authorization and sends acknowledgements unless
quiet add has been chosen (which requires the
quiet\_subscription setting to be "optional") or forced (which
requires the quiet\_subscription setting to be "on").

## Attributes

See also ["Attributes" in Sympa::Request](./Sympa-Request.3.md#attributes).

- {email}

    _Mandatory_.
    E-mail of the user to be added.

- {force}

    _Optional_.
    If true value is specified,
    users will be added even if the list is closed.

- {gecos}

    _Optional_.
    Display name of the user to be added.

- {quiet}

    _Optional_.
    Don't notify addition to the user.

# SEE ALSO

[Sympa::Request::Handler](./Sympa-Request-Handler.3.md).

# HISTORY
