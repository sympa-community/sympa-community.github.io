# NAME

Sympa::ConfDef - Definition of site and robot configuration parameters

# DESCRIPTION

This module keeps definition of configuration parameters for site default
and each robot.

## Global variable

- @params

    Includes following items in order parameters are shown.

    - `{ gettext_id => TITLE }`

        Title for the group of parameters following.

    - `{ name => NAME, DEFINITIONS, ... }`

        Definition of parameter.  DEFINITIONS may contain following pairs.

        - name => NAME

            Name of the parameter.

        - file => FILE

            Conf file where the parameter is defined.  If omitted, the
            parameter won't be added automatically to the config file, even
            if a default is set.
            `"wwsympa.conf"` is a synonym of `"sympa.conf"`.  It remains there
            in order to migrating older versions of config.

        - default => VALUE

            Default value.
            DON'T SET AN EMPTY DEFAULT VALUE! It's useless
            and can lead to errors on fresh install.

        - gettext\_id => STRING

            Description of the parameter.

        - gettext\_comment => STRING

            Additional advice concerning the parameter.

        - sample => STRING

            FIXME FIXME

        - edit => 1|0

            This defines the parameters to be edited.

        - optional => 1|0

            FIXME FIXME

        - vhost => 1|0

            If 1, the parameter can have a specific value in a
            virtual host.

        - db => OPTION

            'db\_first', 'file\_first' or 'no'.

        - obfuscated => 1|0

            FIXME FIXME

        - multiple => 1|0

            If 1, the parameter can have multiple values. Default is 0.

        - scenario => 1|0

            If 1, the parameter is the name of scenario.

# SEE ALSO

[sympa.conf(5)](./sympa.conf.5.md), [robot.conf(5)](./robot.conf.5.md).
