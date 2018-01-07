---
title: 'upgrade_sympa_password(1)'
---

# NAME

upgrade\_sympa\_password, upgrade\_sympa\_password.pl -
Upgrading password in database

# DESCRIPTION

Version later than 5.4 uses MD5 hash instead of
symmetric encryption to store password.

This require to rewrite password in database. This upgrade IS NOT
REVERSIBLE.

# HISTORY

As of Sympa 3.1b.7, passwords may be stored into user table with encrypted
form by reversible RC4.

Sympa 5.4 or later uses MD5 one-way hash function to encode user passwords.
