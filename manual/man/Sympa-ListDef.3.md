---
title: 'Sympa::ListDef(3)'
---

# NAME

Sympa::ListDef - Definition of list configuration parameters

# DESCRIPTION

This module keeps definition of configuration parameters for each list.

## Global variable

- %alias

    Deprecated by Sympa 6.2.16.

- %pinfo

    This hash COMPLETELY defines ALL list parameters.
    It is then used to load, save, view, edit list config files.

    List parameters format accepts the following keywords :

    - format

        Regexp applied to the configuration file entry.
        Or arrayref containing all possible values of parameter.

        Or, if the parameter is paragraph, value of this item is a hashref containing
        definitions of sub-parameters.

        See also ["Node types" in Sympa::List::Config](./Sympa-List-Config.3.md#node-types).

    - format\_s

        Template of regexp applied to the configuration file entry;
        see also ["format"](#format).

        Subpatterns `$word` indicate the name of pattern defined in
        [Sympa::Regexps](./Sympa-Regexps.3.md).

        This was introduced on Sympa 6.2.19b.2.

    - file\_format

        Config file format of the parameter might not be
        the same in memory.

    - split\_char

        Character used to separate multiple parameters.
        Used with the set or the array of scalars.

    - length

        Length of a scalar variable ; used in web forms.

    - scenario

        Tells that the parameter is a scenario, providing its name.

    - default

        Default value for the param ; may be a robot configuration
        parameter (conf).

        If occurrence is `0-1` or `0-n`,
        default value will be assigned
        only when list is created or new node is added to configuration.

    - synonym

        Defines synonyms for parameter values (for compatibility
        reasons).

    - gettext\_unit

        Unit of the parameter ; this is used in web forms and refers
        to translated
        strings in NLS catalogs.

    - occurrence

        Occurrence of the parameter in the config file
        possible values: `0-1`, `1`, `0-n` and `1-n`.
        Example: A list may have multiple owner.

        See also ["Node types" in Sympa::List::Config](./Sympa-List-Config.3.md#node-types).

    - gettext\_id

        Title reference in NLS catalogs.

    - gettext\_comment

        Description text of a parameter.

    - group

        Group of parameters.

    - obsolete

        Obsolete parameter ; should not be displayed
        nor saved.

        As of 6.2.16, if the value is true value and is not `1`,
        defines parameter alias name mainly for backward compatibility.

    - obsolete\_values

        **Deprecated**.

        Defined obsolete values for a parameter.
        These values should not get proposed on the web interface
        edition form.

    - order

        Order of parameters within paragraph.

    - internal

        Indicates that the parameter is an internal parameter
        that should always be saved in the config file.

    - field\_type

        Used to special treatment of parameter value to show it.

        - `'dayofweek'`

            Day of week, `0` - `6`.

        - `'lang'`

            Language tag.

        - `'password'`

            The value to be concealed.

        - `'reception'`

            Reception mode of list member.

        - `'status'`

            Status of list.

        - `'listtopic'`

            List topic.

        - `'unixtime'`

            The time in second from Unix epoch.

        - `'visibility'`

            Visibility mode of list member.

        Most of field types were introduced on Sympa 6.2.17.

    - filters

        See ["Filters" in Sympa::List::Config](./Sympa-List-Config.3.md#filters).

        Introduced on Sympa 6.2.17.

    - validations

        See ["Validations" in Sympa::List::Config](./Sympa-List-Config.3.md#validations).

        Introduced on Sympa 6.2.17.

    - privilege

        _Dynamically assigned_.
        Privilege for specified user:
        `'write'`, `'read'` or `'hidden'`.

        Introduced on Sympa 6.2.17.

    - enum

        _Automatically assigned_.
        TBD.

        Introduced on Sympa 6.2.17.

# SEE ALSO

[config(5)](./config.5.md),
[Sympa::List::Config](./Sympa-List-Config.3.md),
[Sympa::ListOpt](./Sympa-ListOpt.3.md).

# HISTORY

[Sympa::ListDef](./Sympa-ListDef.3.md) was separated from [List](https://metacpan.org/pod/List) module on Sympa 6.2.
