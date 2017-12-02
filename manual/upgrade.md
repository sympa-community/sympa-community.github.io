---
title: 'Upgrading Sympa'
prev: install.md
up: ./
next: admin.md
---

Upgrading Sympa
===============

Upgrading process overview
--------------------------

  1. Preparation.

  2. Stop the services.

  3. Back up everything.

  4. Install new version of Sympa.

  5. Upgrade data.

  6. Restart services.

Preparation
-----------

First, read the following notes:

  - [Release Notes](https://github.com/sympa-community/sympa/blob/sympa-6.2/NEWS.md).
  - [Upgrading notes](upgrade/notes.md).

If imcompatible changes had been made, these notes may state them and
describe measures you can take.

Stop the services
-----------------

See also "[Stopping services](admin/services.md#stopping-services)".

  * Sympa services should be stopped.

  * HTTP server should be stopped.

    On nginx, WWSympa FastCGI service should be stopped at least: nginx itself
    may not be stopped.

  * Mail transfer agent (MTA) is recommended to be stopped.
    If it was not stopped, incoming messages will be queued into the spools of
    Sympa and processed when the services will restart.

  * Note that database service _must not_ be stopped during upgrading process.

Back up everything
------------------

It is recommended to back up all configurations and database in advance.

  * Configurations are saved under following paths.

      - Main configuration file ([``sympa.conf``](layout.md#config)).
      - Global configuration ([``$SYSCONFDIR``](layout.md#sysconfdir)).
      - List home ([``$EXPLDIR``](layout.md#expldir)).

  * To back up database, please consult the documentation of database server.

  * Contrary to expectations, default settings, especially default templates,
    are frequently changed on release by release.  Backing up files under
    [``$DEFAULTDIR``](layout.md#defaultdir) is good idea.

Install new version of Sympa
----------------------------

### Binary distributions

Update all packages you have installed, including Sympa itself.
See the documentaion of each distribution to know how to update packages.

### General instruction

  1. Install new version of Sympa distribution according to description in
     "[Install Sympa distribution](install/install-sympa-distribution.md)".

  2. Update dependent modules if necessary according to description in
     "[Install dependent modules](install/install-dependent-modules.md)".

Upgrade data
------------

  1. If you have installed Sympa from source distribution, run:
     ```bash
     # sympa.pl --upgrade
     ```
     This will upgrade database schema and reload configuration caches
     (On binary releases, this step will be done automatically).

  2. If the Release Notes suggest any manual adjustments, carry out them.

Restart services
----------------

See also "[Starting services](admin/services.md#starting-services)".

Restart services and check if the system will work properly.

