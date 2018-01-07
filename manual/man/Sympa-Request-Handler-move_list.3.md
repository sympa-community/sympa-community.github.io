---
title: 'Sympa::Request::Handler::move_list(3)'
---

# NAME

Sympa::Request::Handler::move\_list - move\_list request handler

# DESCRIPTION

Renames a list or move a list to possibly beyond another virtual host.

On copy mode, Clone a list config including customization, templates,
scenario config but without archives, subscribers and shared.

## Attributes

See also ["Attributes" in Sympa::Request](./Sympa-Request.3.md#attributes).

- {context}

    Context of request.  The robot the new list will belong to.

- {current\_list}

    Source of moving or copying.  An instance of [Sympa::List](./Sympa-List.3.md).

- {listname}

    The name of the new list.

- {mode}

    _Optional_.
    If it is set and its value is `'copy'`,
    won't erase source list.

# SEE ALSO

[Sympa::Request::Collection](./Sympa-Request-Collection.3.md),
[Sympa::Request::Handler](./Sympa-Request-Handler.3.md),
[Sympa::Spindle::ProcessRequest](./Sympa-Spindle-ProcessRequest.3.md).

# HISTORY

[Sympa::Request::Handler::move\_list](./Sympa-Request-Handler-move_list.3.md) appeared on Sympa 6.2.23b.
