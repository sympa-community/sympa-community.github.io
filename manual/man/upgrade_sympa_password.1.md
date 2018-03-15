---
title: 'upgrade_sympa_password(1)'
---

# NAME

upgrade\_sympa\_password, upgrade\_sympa\_password.pl -
Upgrading password in database

# DESCRIPTION

Versions later than 5.4 uses MD5 hash instead of
symmetric encryption to store password.

Versions later than 6.2.26 support bcrypt.

This upgrade requires to rewriting user password entries in the database.
This upgrade IS NOT REVERSIBLE.

# HISTORY

As of Sympa 3.1b.7, passwords may be stored into user table with encrypted
form by reversible RC4.

Sympa 5.4 or later uses MD5 one-way hash function to encode user passwords.

Sympa 6.2.26 or later has optional support for bcrypt.
