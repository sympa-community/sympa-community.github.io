---
title: 'Sympa::ModDef(3)'
---

# NAME

Sympa::ModDef - Definition of dependent modules

# DESCRIPTION

This module keeps definition of modules required by Sympa.

## Global variable

- %cpan\_modules

    This defines the modules.
    Each item has Perl package name as key and hashref containing pairs below
    as value.

    - required\_version => STRING

        Minimum version of package.
        Assume required\_version = '1.0' if not specified.

    - package\_name => STRING

        Name of CPAN module.

    - mandatory => 1|0

        If 1, the module is mandatory.  Default is 0.

    - gettext\_id => STRING

        Usage of this package,

    - gettext\_comment => STRING

        Description of prerequisites if any.

# SEE ALSO

sympa\_wizard(1).
