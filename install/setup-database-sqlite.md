Setup database: SQLite
======================

Requirements
------------

  * Ensure that SQLite 3.x is installed.  As of Sympa-6.2a.33, SQLite 2.x
    and earlier are no longer supported.

  * Install perl-DBD-SQLite package.

General instruction
-------------------

  * Ensure that "db_name" in sympa.conf is absolute path to database file
    you want to create.

  * Create database file and table structure:
    ```
    # touch <db_name>
    # chown sympa:sympa <db_name>
    # sympa.pl --health_check
    ```

