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
     [``db_host``](../man/sympa.conf.5.md#db_host),
     [``db_user``](../man/sympa.conf.5.md#db_user),
     [``db_passwd``](../man/sympa.conf.5.md#db_passwd) and
     [``db_env``](../man/sympa.conf.5.md#db_env).

       * ``db_name`` must be same as Oracle SID.

       * ``db_env`` should include definition of NLS_LANG and ORACLE_HOME.

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
     ```
     # sympa.pl --health_check
     ```

