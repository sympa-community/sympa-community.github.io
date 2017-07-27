Setup database: PostgreSQL
==========================

Requirements
------------

  * PostgreSQL 7.4 or later is required.

  * Install perl-DBD-Pg package.

General instruction
-------------------

  * Ensure that sympa.conf includes appropriate values for these parameters:
    db_type, db_name, db_host, db_user, db_passwd.

  * Create database and role:
    ```
    $ psql
    postgres=# CREATE ROLE <db_user> NOSUPERUSER NOCREATEDB NOCREATEROLE
    postgres=# NOINHERIT LOGIN ENCRYPTED PASSWORD '<db_passwd>';
    postgres=# CREATE DATABASE sympa OWNER <db_user> ENCODING 'UNICODE';
    postgres=# \q
    ```

  * Create table structure:
    ```
    # sympa.pl --health_check
    ```

