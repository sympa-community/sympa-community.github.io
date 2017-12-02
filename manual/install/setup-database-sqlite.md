---
title: 'Setup database: SQLite'
prev: generate-initial-configuration.md
up: setup-database.md
next: configure-system-log.md
---

Setup database: SQLite
======================

Requirements
------------

  * Ensure that SQLite 3.x is installed.  As of Sympa-6.2a.33, SQLite 2.x
    and earlier are no longer supported.

  * Install [DBD-SQLite](https://metacpan.org/release/DBD-SQLite) package.

General instruction
-------------------

  1. Ensure that [``sympa.conf``](../layout.md#config) includes appropriate
     values for these parameters:
     [``db_type``](../man/sympa.conf.5.md#db_type),
     [``db_name``](../man/sympa.conf.5.md#db_name) and
     [``db_timeout``](../man/sympa.conf.5.md#db_timeout) (optional).

       * ``db_type`` must be ``SQLite``.

       * ``db_name`` must be absolute path to database file you want to
         create.

  2. Create database file and table structure:
     ```
     # touch <db_name>
     # chown sympa:sympa <db_name>
     # sympa.pl --health_check
     ```

