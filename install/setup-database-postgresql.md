Setup database: PostgreSQL
==========================

Requirements
------------

* PostgreSQL 7.4 or later is required.

* Install [DBD-Pg](https://metacpan.org/release/DBD-Pg) package.

General instruction
-------------------

1. Ensure that sympa.conf includes appropriate values for these parameters:
   [``db_type``](../man/sympa.conf.5.md#db_type), [``db_name``](../man/sympa.conf.5.md#db_name), [``db_host``](../man/sympa.conf.5.md#db_host), [``db_user``](../man/sympa.conf.5.md#db_user), [``db_passwd``](../man/sympa.conf.5.md#db_passwd).

2. Create database and role:
   ```
   $ psql
   postgres=# CREATE ROLE <db_user> NOSUPERUSER NOCREATEDB NOCREATEROLE
   postgres=# NOINHERIT LOGIN ENCRYPTED PASSWORD '<db_passwd>';
   postgres=# CREATE DATABASE sympa OWNER <db_user> ENCODING 'UNICODE';
   postgres=# \q
   ```

3. Create table structure:
   ```
   # sympa.pl --health_check
   ```

