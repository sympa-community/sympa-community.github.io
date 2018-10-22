---
title: 'Setup database: Oracle Database'
prev: generate-initial-configuration.md
up: setup-database.md
next: configure-system-log.md
---

Setup database: Oracle Database
===============================

Requirements
------------

  * Oracle 7 or later may work.  Oracle 9i or later is recommended.

  * Install [DBD-Oracle](https://metacpan.org/release/DBD-Oracle) package.

General instruction
-------------------

  1. Ensure that [``sympa.conf``](../layout.md#config) includes appropriate
     values for these parameters:
     [``db_type``](../man/sympa.conf.5.md#db_type),
     [``db_name``](../man/sympa.conf.5.md#db_name),
     [``db_host``](../man/sympa.conf.5.md#db_host) (optional),
     [``db_port``](../man/sympa.conf.5.md#db_port) (optional),
     [``db_user``](../man/sympa.conf.5.md#db_user),
     [``db_passwd``](../man/sympa.conf.5.md#db_passwd) and
     [``db_env``](../man/sympa.conf.5.md#db_env).

       * ``db_type`` must be ``Oracle``.

       * ``db_name`` and related parameters may be chosen with one of
         following three methods:

          * Specifying instance with system identifier (SID):

            ``db_name`` specifies SID.
            ``db_host`` must be set
            (if ``db_port`` is not set, `1521` is used as port).

            Example:
            ``` code
            db_name ORCL
            db_host localhost
            ```

          * Specifying instance(s) with local naming:

            ----
            Note:

              * Local naming is available on Sympa 6.2.37b.2 or later.

            ----

            ``db_name`` specifies net service name.
            ``db_host`` must be `none` or must not be set.

            Example:
            ``` code
            db_name net_service_name
            ```

            `tnsnames.ora` file on Sympa server has to include an entry for
            specified net service name.
            `tnsnames.ora` file is usually placed in the directory
            `$ORACLE_HOME/network/admin`
            (If you want to use this file in another place, specify the
            directory by adding `TNS_ADMIN` environment variable to `db_env`
            parameter described in below).

          * Specifying connection with easy connection identifier:

            ----
            Note:

              * Use of connection identifier is available on
                Sympa 6.2.37b.2 or later.

            ----

            ``db_name`` specifies easy connection identifier.
            ``db_host`` must be `none` or must not be set.

            Examples:
            ``` code
            db_name db.example.org/service_name
            ```
            ``` code
            db_name db.example.org:1521/name.dom.ain
            ```

       * ``db_env`` should include definition of NLS_LANG and ORACLE_HOME.
         Character set should be `AL32UTF8`.

     Example:
     ``` code
     db_type Oracle
     db_name ORCL
     db_host localhost
     db_user sympa
     db_passwd (secret)
     db_env NLS_LANG=American_America.AL32UTF8;ORACLE_HOME=/u01/app/oracle/product/11.2.0/dbhome_1
     ```

  2. Create database user:
     ```
     $ NLS_LANG=American_America.AL32UTF8; export NLS_LANG
     $ ORACLE_HOME=<Oracle home>; export ORACLE_HOME
     $ ORACLE_SID=<Oracle SID>; export ORACLE_SID
     $ sqlplus <system login>/<password>
     SQL> CREATE USER <db_user> IDENTIFIED BY <db_passwd>
       2  DEFAULT TABLESPACE <tablespace>
       3  TEMPORARY TABLESPACE <tablespace>;
     SQL> GRANT CREATE SESSION TO <db_user>;
     SQL> GRANT CREATE TABLE TO <db_user>;
     SQL> GRANT CREATE SYNONYM TO <db_user>;
     SQL> GRANT CREATE VIEW TO <db_user>;
     SQL> GRANT EXECUTE ANY PROCEDURE TO <db_user>;
     SQL> GRANT SELECT ANY TABLE TO <db_user>;
     SQL> GRANT SELECT ANY SEQUENCE TO <db_user>;
     SQL> GRANT RESOURCE TO <db_user>;
     SQL> QUIT
     ```

  3. Create table structure:
     ``` bash
     # sympa.pl --health_check
     ```

