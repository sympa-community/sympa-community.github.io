# NAME

Sympa::ListOpt - Definition of list configuration parameter values

# DESCRIPTION

[Sympa::ListOpt](./Sympa-ListOpt.3.md) gives information about options used for values of list
configuration.

## Function

- get\_option\_description ( $that, $value, \[ $type, \[ $withval \] \] )

    _Function_.
    Gets i18n-ed title of option.
    Language context must be set in advance (See [Sympa::Language](./Sympa-Language.3.md)).

    Parameters:

    - $that

        Context, instance of [Sympa::List](./Sympa-List.3.md), Robot or Site.

    - $value

        Value of option.

    - $type

        Type of option:
        field\_type (see [Sympa::ListDef](./Sympa-ListDef.3.md))
        or other (list config option, default).

    - $withval

        Adds value of option to returned title.

    Returns:

    I18n-ed title of option value.

# SEE ALSO

[Sympa::Language](./Sympa-Language.3.md),
[Sympa::ListDef](./Sympa-ListDef.3.md).

# HISTORY

[Sympa::ListOpt](./Sympa-ListOpt.3.md) appeared on Sympa 6.2.13.
