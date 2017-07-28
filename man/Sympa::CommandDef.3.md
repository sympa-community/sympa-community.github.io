# NAME

Sympa::CommandDef - Definition of mail commands

# SYNOPSIS

TBD

# DESCRIPTION

This module keeps definition of mail commands.

## Global variable

- %comms

    This hash defines format of mail commands.
    It is used for decoding and encoding between command lines and internal
    request objects.

    Key is the name of action which is given as `action` parameter to constructor
    of [Sympa::Request](./Sympa::Request.3.md).
    Note that not all sort of requests are defined.

    Value is the hashref.
    Each item of hashrefs accepts the following keywords :

    - cmd\_regexp

        A regexp matching command.
        Note that `i` modifier is necessary.

    - arg\_regexp

        A regexp matching command line arguments.
        Note that `i` modifier may be needed.

    - arg\_keys

        An arrayref of parameter names mapping command line to attribute.
        `'localpart'` is special:
        If it is contained, `context` attribute of resulting request object is
        an instance of [Sympa::List](./Sympa::List.3.md) class.

    - cmd\_format

        A string to format command line using attributes.
        If this item is code reference, it will be called with request object
        and returned value will be used as format string.

    - filter

        A coderef to perform additional checking.
        It is called with request object and, if it returns false value,
        decoding will fail.

# SEE ALSO

[Sympa::Request](./Sympa::Request.3.md), [Sympa::Request::Message](./Sympa::Request::Message.3.md).

# HISTORY

[Sympa::CommandDef](./Sympa::CommandDef.3.md) appeared on Sympa 6.2.13.
