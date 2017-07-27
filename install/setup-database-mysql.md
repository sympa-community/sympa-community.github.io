Setup database: MySQL or MariaDB
================================

Requirements
------------

  * MySQL 4.1.1 or later, and any releases of MariaDB will work.

  * Install perl-DBD-MySQL package.

  * MyISAM or Aria, and InnoDB or XtraDB storage engines may work.  Check
    ``default-storage-engine`` and ``default-table-type`` options of your
    MySQL / MariaDB server.

General instruction
-------------------

  * Ensure that sympa.conf includes appropriate values for these parameters:
    db_type, db_name, db_host, db_user and db_passwd.

  * Create database and database user:
    ```
    $ mysql
    mysql> CREATE DATABASE sympa CHARACTER SET utf8;
    mysql> GRANT ALL PRIVILEGES ON sympa.* TO <db_user>@<client host>
        -> IDENTIFIED BY '<db_passwd>';
    mysql> QUIT
    ```

  * Create table structure:
    ```
    # sympa.pl --health_check
    ```

N.B.: MySQL/MariaDB 5.5.3 or later provides ``utf8mb4`` character set
which covers full range of Unicode including such as chinese ideographs
used for persons' names.  As of Sympa-6.2a.33 r8753, both ``utf8`` and
``utf8mb4`` character sets are supported.  To use ``utf8mb4`` charcter
set, you might want to replace ``utf8`` in SQL statement above with
``utf8mb4``.

