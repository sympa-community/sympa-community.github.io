---
title: 'sympa_wizard(1)'
---

# NAME

sympa\_wizard, sympa\_wizard.pl - Help Performing Sympa Initial Setup

# SYNOPSIS

`sympa_wizard.pl`
\[ `--batch` \[ _key_=_value_ ... \] \]
\[ `--check` \]
\[ `--create` \[ `--target=`_file_ \] \]
\[ `--display` \]
\[ `-h, --help` \]
\[ `-v, --version` \]

# OPTIONS

- `sympa_wizard.pl`

    Edit current Sympa configuration.

- `sympa_wizard.pl` `--batch` _key_=_value_ ...

    Edit in batch mode.
    Arguments would include pairs of parameter name and value.

- `sympa_wizard.pl` `--check`

    Check CPAN modules needed for running Sympa.

- `sympa_wizard.pl` `--create` \[ `--target` _file_ \]

    Creates a new `sympa.conf` configuration file.

- `sympa_wizard.pl` `--display`

    Outputs all configuration parameters.

- `sympa_wizard.pl` `--help`

    Display usage instructions.

- `sympa_wizard.pl` `--version`

    Print version number.

# HISTORY

This program was originally written by:

- Serge Aumont <sa@cru.fr>
- Olivier Salaün <os@cru.fr>

`--batch` and `--display` options are added on Sympa 6.1.25 and 6.2.15.
