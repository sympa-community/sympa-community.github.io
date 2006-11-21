Moving to another server
------------------------

If you're upgrading and moving to another server at the same time, we recommend you first to stop the operational service, move your data and then upgrade Sympa on the new server. This will guarantee that Sympa upgrade procedures have been applied on the data.

The migration process requires that you move the following data from the old server to the new one :

  - the user database. If using mysql you can probably just stop `mysqld` and copy the `/var/lib/mysql/sympa/` directory to the new server.
  - the `/usr/local/sympa-os/expl` directory that contains list config
  - the  directory that contains the spools
  - the  directory and `/usr/local/sympa-os/etc/sympa.conf` and `wwsympa.conf`. Sympa new installation create a file `/usr/local/sympa-os/etc/sympa.conf` (see [7](node8.html#exp-admin)) and initialize randomly the cookie parameter. Changing this parameter will break all passwords. When upgrading Sympa on a new server take care that you start with the same value of this parameter, otherwise you will have troubles !
  - the web archives

In some case, you may want to install the new version and run it for a few days before switching the existing service to the new Sympa server. In this case perform a new installation with an empty database and play with it. When you decide to move the existing service to the new server :

  1. stop all sympa processus on both servers,
  2. transfert the database
  3. edit the `/data_structure.version` on the new server ; change the version value to reflect the old number
  4. start `sympa.pl -upgrade`, it will upgrade the database structure according the hop you do.

