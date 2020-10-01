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
     [``db_type``](/gpldoc/man/sympa_config.5.html#db_type),
     [``db_name``](/gpldoc/man/sympa_config.5.html#db_name) and
     [``db_timeout``](/gpldoc/man/sympa_config.5.html#db_timeout) (optional).

       * ``db_type`` must be ``SQLite``.

       * ``db_name`` must be absolute path to database file you want to
         create.

       Example:
       ``` code
       db_type SQLite
       db_name /var/lib/sympa/sympa.sqlite
       ```

  2. Create database file and table structure:
     ``` bash
     # touch <db_name>
     # chown sympa:sympa <db_name>
     # sympa.pl --health_check
     ```

Instruction for earlier releases of Sympa
-----------------------------------------

----
Note:

  * This section describes instruction with Sympa prior to 6.2.

----

  1. Set appropriate parameters in `sympa.conf` as described in above.

  2. Create database file (note that `<db_name>` is the full path to database
     file you want to create).

     ``` bash
     # touch <db_name>
     # chown sympa:sympa <db_name>
     ```
  3. Create table structure (Note: replace
     [``$SCRIPTDIR``](../layout.md#scriptdir)):

     ``` bash
     $ sqlite3 <db_name>
     sqlite> .read $SCRIPTDIR/create_db.SQLite
     sqlite> .quit
     ```

