---
title: 'Setup database: MySQL or MariaDB'
prev: generate-initial-configuration.md
up: setup-database.md
next: configure-system-log.md
---

Setup database: MySQL or MariaDB
================================

Requirements
------------

  * MySQL 4.1.1 or later, and any releases of MariaDB will work.

  * Install [DBD-mysql](https://metacpan.org/release/DBD-mysql) package.

  * MyISAM or Aria, and InnoDB or XtraDB storage engines may work.  Check
    ``default-storage-engine`` and ``default-table-type`` options of your
    MySQL / MariaDB server.

General instruction
-------------------

  1. Ensure that [``sympa.conf``](../layout.md#config) includes appropriate
     values for these parameters:
     [``db_type``](../man/sympa.conf.5.md#db_type),
     [``db_name``](../man/sympa.conf.5.md#db_name),
     [``db_host``](../man/sympa.conf.5.md#db_host),
     [``db_port``](../man/sympa.conf.5.md#db_host) (optional),
     [``db_user``](../man/sympa.conf.5.md#db_user) and
     [``db_passwd``](../man/sympa.conf.5.md#db_passwd) (optional).

       * ``db_type`` must be ``MySQL`` or ``mysql`` (obsoleted).

       * If ``db_host`` is set to ``localhost``, Sympa will connect to
         database on local machine using Unix domain socket.  Otherwise, if
         another host name or IP address is set, TCP socket will be used (If
        ``db_port`` is not specified, ``3306`` will be used as port).

       Example:
       ``` code
       db_type MySQL
       db_name sympa
       db_host localhost
       db_user sympa
       db_passwd (secret)
       ```

  2. Create database and database user:
     ```
     $ mysql
     mysql> CREATE DATABASE sympa CHARACTER SET utf8;
     mysql> GRANT ALL PRIVILEGES ON sympa.* TO <db_user>@<client host>
         -> IDENTIFIED BY '<db_passwd>';
     mysql> QUIT
     ```

  3. Create table structure:
     ``` bash
     # sympa.pl --health_check
     ```

----
Note:

  * MySQL/MariaDB 5.5.3 or later provides ``utf8mb4`` character set
    which covers full range of Unicode including such as Chinese ideographs
    used for persons' names.  As of Sympa-6.2a.33 r8753, both ``utf8`` and
    ``utf8mb4`` character sets are supported.  To use ``utf8mb4`` character
    set, you might want to replace ``utf8`` in SQL statement above with
    ``utf8mb4``.

----

