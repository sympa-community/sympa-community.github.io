---
title: 'Setup database: PostgreSQL'
prev: generate-initial-configuration.md
up: setup-database.md
next: configure-system-log.md
---

Setup database: PostgreSQL
==========================

Requirements
------------

  * PostgreSQL 7.4 or later is required.

  * Install [DBD-Pg](https://metacpan.org/release/DBD-Pg) package.

General instruction
-------------------

  1. Ensure that [``sympa.conf``](../layout.md#config) includes appropriate
     values for these parameters:
     [``db_type``](/gpldoc/man/sympa.conf.5.html#db_type),
     [``db_name``](/gpldoc/man/sympa.conf.5.html#db_name),
     [``db_host``](/gpldoc/man/sympa.conf.5.html#db_host),
     [``db_port``](/gpldoc/man/sympa.conf.5.html#db_port) (optional),
     [``db_user``](/gpldoc/man/sympa.conf.5.html#db_user) and
     [``db_passwd``](/gpldoc/man/sympa.conf.5.html#db_passwd) (optional).

       * ``db_type`` must be ``PostgreSQL`` or ``Pg`` (obsoleted).

       * ``db_host`` may be host name or IP address of the database server
         with TCP connection (if ``db_port`` is not specified, ``5432`` is
         used as port), or directory of the Unix domain sockets (sockets must
         be writable by ``sympa`` group).

       Example:
       ``` code
       db_type PostgreSQL
       db_name sympa
       db_host localhost
       db_user sympa
       db_passwd (secret)
       ```

  2. Create database and role:
     ```
     $ psql
     postgres=# CREATE ROLE <db_user> NOSUPERUSER NOCREATEDB NOCREATEROLE
     postgres=# NOINHERIT LOGIN ENCRYPTED PASSWORD '<db_passwd>';
     postgres=# CREATE DATABASE sympa OWNER <db_user> ENCODING 'UNICODE';
     postgres=# \q
     ```

  3. Create table structure:
     ``` bash
     # sympa.pl --health_check
     ```

Instruction for earlier releases of Sympa
-----------------------------------------

----
Note:

  * This section describes instruction with Sympa prior to 6.2.

----

  1. Set appropriate parameters in `sympa.conf` as described in above.

  2. Create database and role as described in above.

  3. Create table structure (Note: replace
     [``$SCRIPTDIR``](../layout.md#scriptdir)):

     ``` bash
     $ psql -U <db_user> -d sympa < $SCRIPTDIR/create_db.Pg
     ```

