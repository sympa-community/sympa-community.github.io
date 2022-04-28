---
title: 'Moving to another server'
prev: in-place.md
up: ../upgrade.md
next: notes.md
---

Moving to another server
========================

If you're upgrading and moving to another server at the same time, we recommend you first to stop the operational service, move your data and then upgrade Sympa on the new server. This will guarantee that Sympa upgrade procedures have been applied on the data.

The migration process requires that you move the following data from the old server to the new one:

  - The user database. For example if you are using MySQL, you can probably just stop `mysqld` and copy the database directory to the new server.
  - The [``$EXPLDIR``](../layout.md#expldir) directory that contains list configurations.
  - The [``$SPOOLDIR``](../layout.md#spooldir) directory that contains the spools.
  - The [``sympa.conf``](../layout.md#config), and the file `wwsympa.conf`
    if it exists.

    ----
    **Note**:

    If you plan to upgrade to Sympa 6.2.60 _or earlier_,
    make sure that the `cookie` parameter in `sympa.conf` won't be changed.
    Some distributions randomly initialize this parameter in new installation.

    When upgrading Sympa on a new server, take care that you start with the
    same value of this parameter, otherwise you might have problems,
    such as breakage of all passwords for web interface.

    The note above won't be applied to Sympa 6.2.62 or later which no longer
    uses the `cookie` parameter.

    ----
  - The [``$SYSCONFDIR``](../layout.md#sysconfdir) directory that contains customized scenarios, templates and so on.
  - The [``$ARCDIR``](../layout.md#arcdir) directory that contains archives.

In some cases, you may want to install the new version and run it for a few days before switching the existing service to the new Sympa server. In this case, perform a new installation with an empty database and play with it.

When you decide to move the existing service to the new server:

  1. Stop all sympa processes on both servers.
  2. Transfer all the data described in above from the old server.
  3. Check "[Upgrading notes](notes.md)" and make appropriate manual
     adjustments as necessity.
  4. Edit the `data_structure.version` file on the new server to change the version value to reflect the old number.
  5. Run "``sympa.pl --upgrade``".
     It will upgrade the database structure and several configuration files
     according to the hop you do.

