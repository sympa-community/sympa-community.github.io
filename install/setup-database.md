---
title: 'Setup database'
prev: generate-initial-configuration.md
up: ../install.md
next: configure-system-log.md
---

Setup database
==============

Database setup overview
-----------------------

1. Appropriate DBI driver (DBD) suitable for chosen RDBMS should be installed:
   For example DBD-mysql, DBD-Pg, DBD-Oracle, DBD-Sybase or DBD-SQLite.

2. Create a database and a database user dedicated for Sympa.  The user
   must have most of privileges on database.

Instruction by RDBMSs
---------------------

- [MySQL or MariaDB](setup-database-mysql.md)
- [Oracle Database](setup-database-oracle.md)
- [PostgreSQL](setup-database-postgresql.md)
- [SQLite](setup-database-sqlite.md)

