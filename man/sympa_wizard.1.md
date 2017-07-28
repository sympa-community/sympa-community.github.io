# NAME

sympa\_wizard, sympa\_wizard.pl - Help Performing Sympa Initial Setup

# SYNOPSIS

**sympa\_wizard.pl**
    \[**--batch** \[_key_=_value_ ...\]\]
    \[**--check**\]
    \[**--create** \[**--target=**_file_\]\]
    \[**--display**\]
    \[**-h, --help**\]
    \[**-v, --version**\]

# OPTIONS

- sympa\_wizard.pl

    Edit current Sympa configuration.

- sympa\_wizard.pl --batch _key_=_value_ ...

    Edit in batch mode.
    Arguments would include pairs of parameter name and value.

- sympa\_wizard.pl --check

    Check CPAN modules needed for running Sympa.

- sympa\_wizard.pl --create \[--target file\]

    Creates a new `sympa.conf` configuration file.

- sympa\_wizard.pl --display

    Outputs all configuration parameters.

- sympa\_wizard.pl --help

    Display usage instructions.

- sympa\_wizard.pl --version

    Print version number.

# HISTORY

This program was originally written by:

- Serge Aumont <sa@cru.fr>
- Olivier Salaün <os@cru.fr>

`--batch` and `--display` options are added on Sympa 6.1.25 and 6.2.15.
