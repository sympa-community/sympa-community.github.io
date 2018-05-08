---
title: 'Upgrading notes'
prev: move.md
up: ../upgrade.md
---

Upgrading notes
===============

----
Note:

  * See also "[Upgrading Sympa](../upgrade.md)" for general upgrading
    instruction.

----

Upgrading from Sympa 6.2.x or earlier
-------------------------------------

After release of 6.2, several changes on templates are made.
If you have customized templates with earlier version, you should check if web
interface will work correctly after upgrading, and reapply customization to new
templates as necessity.

Following subsections describe changes by particular versions of 6.2.x.
If you are planning to upgrade from version prior to 6.2, see also sections
below.

### From versions prior to 6.2.30

  * If `sympa.pl --upgrade` crashes due to undefined variable `$pictures_dir`, remove following files and retry:
      - **`sympa.conf.bin`** placed in the same directory as [``sympa.conf``](../layout.md#config) file.
      - **`robot.conf.bin`** placed in the same directory as each ``robot.conf`` file, if any.

### From versions prior to 6.2.28

#### Renamed scenarios

  * `access_web_archive.*` scenarios are renamed to `archive_web_access.*`.
    If you have customized any of these scenarios, you have to rename them
    under config directory.

#### Changes on `css_path` and `css_url` parameters

  * `css_url` and `css_path` parameters in `robot.conf` are no longer
     available: Those in `sympa.conf` (if any) are used.

     If you have specified `css_path` parameter by each `robot.conf`,
     you have to move each directory to subdirectory named by domain
     under directory specified by `css_path` directory in `sympa.conf`
     (by default [``$CSSDIR``](../layout.md#cssdir)).

#### New `configure` options and parameters

  * If you have used none of `--with-staticdir` configure option,
    `static_content_path` parameter nor `static_content_url` parameter,
    no changes described below are required.

  * If you have built Sympa from source and have specified `--with-staticdir`
    configure option, you might want to specify `--with-cssdir` and
    `--with-picturesdir` also.

      - With earlier version:
        ``` bash
        $ ./configure --with-staticdir=DIR (...)
        ```
      - With recent version:
        ``` bash
        $ ./configure --with-staticdir=DIR --with-cssdir=DIR/css --with-picturesdir=DIR/pictures (...)
        ```

  * If you have specified
    [`static_content_path`](../man/sympa.conf.5.md#static_content_path)
    parameter in [``sympa.conf``](../layout.md#config), you might want to
    specify [`css_path`](../man/sympa.conf.5.md#css_path) (if you have not
    specified it) and [`pictures_path`](../man/sympa.conf.5.md#pictures_path)
    also.

      - With earlier version:
        ``` code
        static_content_path DIR
        ```
      - With recent version:
        ``` code
        static_content_path DIR
        css_path            DIR/css
        pictures_path       DIR/pictures
        ```

  * If you have specified
    [`static_content_url`](../man/sympa.conf.5.md#static_content_url)
    parameter in [``sympa.conf``](../layout.md#config), you might want to
    specify [`css_url`](../man/sympa.conf.5.md#css_url) (if you have not
    specified it) and [`pictures_url`](../man/sympa.conf.5.md#pictures_url)
    also.

      - With earlier version:
        ``` code
        static_content_url PATH
        ```
      - With recent version:
        ``` code
        static_content_url PATH
        css_url            PATH/css
        pictures_url       PATH/pictures
        ```

#### New password hash format

It is not forced, but it is recommended to upgrade password storage format of
web interface using `bcrypt`, more secure hash function.  See
"[Upgrading password storage on earlier version](../customize/builtin-auth.md#upgrading-password-storage-on-earlier-version)"
for details.

### From versions prior to 6.2.24

  - Data sources: "`%`" sign in SQL data source no longer need escaping by
    duplicating it, i.e. "`%%`". Administrators on the sites allowing SQL
    data sources are recommended to check data source settings and modify
    them as necessity. 

  - WWSympa: FastCGI support became mandatory. CGI mode was deprecated.
    See "[Configure HTTP server](../install/configure-http-server.md)
    for details about configuration.

### From versions prior to 6.2.18

  - Especially on
    6.2.18, web templates for shared document repository and list configuration
    edit form broke backward compatibility in exchange for bug fixes.

Upgrading from Sympa 6.1.x or earlier
-------------------------------------

Several changes in Sympa 6.2 require to perform specific operations when
upgrading. Here is the ordered list of operations to perform.

  1. [_Source distribution only_] Check the options for ``configure`` script.

     As some Sympa binaries have been renamed, your sympa init script have to
     be updated. Please make sure your configure options have been correctly
     set, so that this script ends up in the correct location.

     See "[Run configure script](../install/install-sympa-distribution-source.md#run-configure-script)"
     for details.

  2. [_Source distribution only_] Run additional commands after
     ``make install``:
     ``` bash
     # sympa_wizard.pl --check            # Update dependent modules
     # sympa.pl --upgrade_config_location # move existing configs to /etc/sympa/ directory
     # sympa.pl --upgrade                 # merge sympa.conf and wwsympa.conf
     ```

  3. Commands after upgrade:

     These are always needed you are using either binary distribution or source
     distribution.
     ``` bash
     # upgrade_bulk_spool.pl              # move messages stored in database to filesystem
     # upgrade_send_spool.pl              # move messages sent through web interface to the new format
     ```
     For details, read manual pages of
     [upgrade_bulk_spool.pl](../man/upgrade_bulk_spool.1.md) and
     [upgrade_send_spool.pl](../man/upgrade_send_spool.1.md).

  4. For all your **custom action** templates, move them into
     **[``$SYSCONFDIR``](../layout.md#sysconfdir)`/custom_actions`**
     directory.

After the process above, the following changes will have occured:

  - The `sympa.conf` is now located in a `sympa/` subdirectory.
  - Content of ``wwsympa.conf`` will have been merged into `sympa.conf`.
    `wwsympa.conf` will no longer be used.
  - The `sympa.conf` remaining will have been stripped of any "`color_*`"
    parameters, to prevent you from having a messed up web interface.

      - Any **`robot.conf`** will have had the same treatment as `sympa.conf`
        (copy, then stripped of `color_*` parameters).

  - Older `sympa.conf` will have need copied to
    "`sympa.conf.upgrade.`_dd_`.`_Month_`.`_yyyy_`-`_hh_`.`_mm_`.`_ss_".
  - Any customized "**`web_tt2`**" directory (either in
    [``$SYSCONFDIR``](../layout.md#sysconfdir)`/` or in
    [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`_robot_`/`)
    will have been renamed to
    "`web_tt2.upgrade.`_dd_`.`_Month_`.`_yyyy_`-`_hh_`.`_mm_`.`_ss_".

    Indeed, as most of the Sympa templates have been changed for the new skin,
    your customizations could not be compliant to the new web interface. You
    should see how to reintroduce them to the new templates.

Additionally, you may have to fix up configuration manually:

  - New parameter:
    [`process_archive`](../man/list_config.md#process_archive) controls
    archiving.
    The default is "`off`": To enable archiving, it must be set to "`on`"
    explicitly (in `sympa.conf`, `robot.conf` or each list `config` file).

  - Renamed list parameters:
    `web_archive.access` to `archive.web_access`;
    `archive.access` to `archive.mail_access`;
    `web_archive.quota` to `archive.quota`;
    `web_archive.max_month` to `archive.max_month`.

  - Deprecated list parameter:
    `user_data_source`.  All subscribers are stored in database.
    The `subscribers` file in list directory will no longer be imported.

  - Administrators are strongly recommended to check new configuration.
    Customized scenarios, list creation templeates and edit_list.conf will
    never be upgraded automatically. 

Upgrading from Sympa 5.4.x or earlier
-------------------------------------

Since there are big changes after 5.4.x, we recommend that recent version
of Sympa will be installed into new machine, or at least will be installed
under separate directory, keeping installation and data of earlier version.

  1. Stop services of earlier version, for example doing:
     ``` bash
     # /etc/rc.d/init.d/sympa stop
     ```
     Then [back up everything](../upgrade.md#back-up-everything).

     Note that you should also backup init script of earlier version, because
     it may be overwritten by installing recent version (if you will install
     recent version to the same machine).

  2. Install recent version of Sympa.

       - See "[Installing Sympa](../install.md)" to install recent version.

  3. Copy configuration files and database:

       - Copy configuration files ``sympa.conf`` _and_ ``wwsympa.conf`` in
         earlier version to recent version.  Then edit them to fix up
         configuration (including name of database below).

       - Queued messages in spools of earlier version should be copied into
         the new location.  Their formats will be changed later.

       - Restore entire database content as a _new_ database (e.g. naming it
         as "`sympa6`" instead of "`sympa`").  Because, succeeding process
         will upgrade content and structure of database and those changes are
         not recoverable.

         ----
         Note:

           * On this occation, you might want to check if database schema
             support Unicode-aware character set, e.g. `utf8`, `UNICODE`,
             `AL32UTF8`. Even if it does not, Sympa will work, however
             it is desirable.  To know how to convert your database to be
             Unicode-aware, please consult to documentation of database
             server.

         ----

  4. Upgrade configuration and data:

       - With recent version of Sympa, run, for example:
         ``` bash
         # sympa.pl --upgrade --from=5.4.7
         ```
         Note that `5.4.7` above must be replaced with the version number you
         are updating from.  Doing this, configuration and database structure
         will be upgraded.

       - Upgrade format of web interface password in the database.  See
         "[Upgrading password storage on earlier version](../customize/builtin-auth.md#upgrading-password-storage-on-earlier-version)".

       - Check customizations of templates, scenarios and list creation
         templates on earlier version,
         and reapply them to recent version if possible.

  5. Start services of recent version (see
     "[Starting services](../admin/services.md#starting-services)"),
     then check if everything goes well.

Upgrading from Sympa prior to 5.0
---------------------------------

(Not yet written)

