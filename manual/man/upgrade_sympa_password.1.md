---
title: 'upgrade_sympa_password(1)'
---

# NAME

upgrade\_sympa\_password, upgrade\_sympa\_password.pl -
Upgrading password in database

# SYNOPSIS

    upgrade_sympa_password.pl [--dry_run|-n] [--debug|d] [--verbose|v] [--config file ] [--cache file] [--nosavecache] [--noupdateuser] [--limit|l number_of_users]

# OPTIONS

- --dry\_run|-n

    Shows what will be done but won't really perform the upgrade process.

- --debug|-d

    Print additional debugging information during the upgrade process.

- --verbose|-v

    Print verbose logging messages during the upgrade process.

- --config FILENAME

    Specify the pathname of the file to use as the Sympa configuration file.
    Otherwise the system default Sympa configuration file is used.

- --cache FILENAME

    Specify the pathname of a file to store precalculated hashes for reuse on
    subsequent runs of the script.

    The file is created if it does not already exist.

    This option is useful for large sites using intentionally expensive
    password hashes such as bcrypt. In that case this script can be run in
    advance to precalculate hashes and reduce the time required during the
    final upgrade process.

    WARNING: since it contains sensitive password data, this file should
    be protected as carefully as any other password file, or a database
    dump of the Sympa user\_table.

- --nosavecache

    Disables updates of the cache. The cache is still consulted if specified with `--cache`.

- --noupdateuser

    Disables updates of the user\_table. Mostly useful when precalculating user
    hashes in advance.

# DESCRIPTION

Versions later than 5.4 use one-way hashes instead of symmetric encryption to
store passwords. This script upgrades any symmetric encrypted passwords it finds to one-way hashes.

Versions later than 6.2.26 support bcrypt.

This upgrade requires to rewriting user password entries in the database.
This upgrade IS NOT REVERSIBLE.

# HISTORY

As of Sympa 3.1b.7, passwords may be stored into user table with encrypted
form by reversible RC4.

Sympa 5.4 or later uses MD5 one-way hash function to encode user passwords.

Sympa 6.2.26 or later has optional support for bcrypt.
