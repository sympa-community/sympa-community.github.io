Upgrading Sympa
===============

Upgrading process overview
--------------------------

1. Preparation.

2. Stop the services.

3. Back up everything.

4. Install new version of Sympa.

5. Upgrade data

6. Restart services.

Preparation
-----------

First, read the [Release Notes](https://github.com/sympa-community/sympa/blob/sympa-6.2/NEWS.md).

If imcompatible changes had been made, the Release Notes may state them and
describe measures you can take.

Stop the services
-----------------

* Sympa services should be stopped.

* HTTP server should be stopped.

  On nginx, WWSympa FCGI service should be stopped at least: nginx itself may
  not be stopped.

* Mail transfer agent (MTA) is recommended to be stopped.
  If it was not stopped, incoming messages will be queued into the spools of
  Sympa and processed when the services will restart.

* Database service _must not_ be stopped during upgrading process.

Back up everything
----=-------------

It is recommended to back up all configurations and database in advance.

* Configurations are saved under following paths.
  - Main configuration file ([``sympa.conf``](layout.md#config)).
  - Global configuration ([``$SYSCONFDIR``](layout.md#sysconfdir)).
  - List home ([``$EXPLDIR``](layout.md#expldir)).

* To backup database, please consult the documentation of database server.

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
   "[Installing Sympa distribution](install/installing-sympa-distribution.md)".

2. Update dependent modules if necessary according to description in
   "[Installing dependent modules](install/installing-dependent-modules.md).

Upgrade data
------------

1. If you have installed Sympa from source distribution, run:
   ```bash
   # sympa.pl --upgrade
   ```
   This will upgrade database schema and reload configuration caches
   (This step will be done automatically by binary releases)

2. If the Release Notes suggest any manual adjustments, carry out them.

Restart services
----------------

Restart services and check if the system will work properly.

