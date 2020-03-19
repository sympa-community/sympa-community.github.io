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

### From version prior to 6.2.56

(Coming later)

  * If you have set [`http_host`](/gpldoc/man/sympa.conf.5.md#http_host)
    parameter in `sympa.conf` or `robot.conf`, you may have to change it.

      - If the value of `http_host` is identical to the host part of
        `wwsympa_url` parameter, it may simply be removed.  For example,
        ``` code
        wwsympa_url       http://web .example.org/sympa
        http_host         web.example.org
        ```
        may be changed to
        ``` code
        wwsympa_url       http://web.example.org/sympa
        ```
      - Otherwise, you have to replace it with appropriate
        [`wwsympa_url_local`](/gpldoc/man/sympa.conf.5.md#wwsympa_url_local)
        parameter.  For example,
        ``` code
        wwsympa_url       http://web.example.org/sympa
        http_host         backend.example.org
        ```
        should be modified as
        ``` code
        wwsympa_url       http://web.example.org/sympa
        wwsympa_url_local http://backend.example.org/sympa
        ```

### From version prior to 6.2.54

  * If you are using the family_signoff link (the URL link in message footer
    that allows unsubscribing from all the lists in a family), you have to
    modify `message.footer.tt2` of the family according to new format (See
    "[Family unsubscription](../customize/basics-families.md#family-unsubscription)"
    for details).


### From version prior to 6.2.50

  * Some scenarios and list creation templates for "intranet" use cases were made
    optional: They have been moved into `samples/`.

    If you have to use following scenario files, copy those files in
    `samples/intranet/scenari/` to appropriate `scenari/` directory:
      - `archive_web_access.intranet`
      - `create_list.intranet`
      - `review.intranet`
      - `send.intranet`
      - `send.intranetorprivate`
      - `subscribe.intranet`
      - `subscribe.intranetorowner`
      - `visibility.intranet`

    If you have to use these files in a list creation template, copy those files
    in `samples/intranet/create_list_templates` to a subdirectory of appropriate
    `create_list_template/` directory:
      - `intranet/comment.tt2`
      - `intranet/config.tt2`

### From version prior to 6.2.48

  * Inclusion feature of members, owners or moderators has been reconstructed.
    Slightly long time can be spent to refresh inclusion of users entirely
    at the first time just after upgrade.

  * Perl version 5.10.1 or later will be supported.

    ----
    Note:

      * As of Sympa 6.2.45b, support for Perl 5.10.0 or earlier has been
        dropped.

    ----

### From version prior to 6.2.44

  * WWSympa: TLS client authentication:
    Now it gets `rfc822Name` in X.509v3 `subjectAltName`, otherwise
    `emailAddress` attribute in subject DN.
    Note that earlier efforts getting attribute such as `MAIL`,
    `Email` in subject DN are no longer supported.

### From versions prior to 6.2.42

  * Authorization schearios:
    The "default" scenario files named `*.default` (regular file or symbolic
    link) are no longer available: Default list scenariios have to be
    specified by parameters in `robot.conf` or `sympa.conf`.

    For details on parameters see
    "[Default privileges for the lists](/gpldoc/man/sympa.conf.5.html#default-privileges-for-the-lists)"
    in `sympa.conf(5)` manual page.
    Previous default settings using symbolic links are automatically migrated
    during upgrading process. However you should review the changes in
    `sympa.conf` (and `robot.conf`).

  * WWSympa and SympaSOAP:

      * If a virtual domain setting does not have `auth.conf`,
        `crawlers_detection.conf` or `trusted_applications.conf` while it is
        there in [`$SYSCONFDIR`](../layout.md#sysconfdir), the latter will be
        used.  Previously in such case, the latter was ignored and only
        built-in authnetication was enabled.
      * Built-in authantication:
        RC4 reversible encryption of password storage in database using
        Crypt::CipherSaber was dropped.  If you have been using it, run
        [``upgrade_sympa_passowrd.pl``](/gpldoc/man/upgrade_sympa_password.1.html) to
        rehash passwords stored in database.

        ----
        Note:

          * Though it is not forced, it is recommended to upgrade password storage
            format using `bcrypt`, more secure hash function.  See
            "[Upgrading password storage on earlier version](../customize/builtin-auth.md#upgrading-password-storage-on-earlier-version)"
            for details.

        ----
      * LDAP authentication:
        Now entry of authenticating user is retrieved by the LDAP account
        specified by `bind_dn` parameter.
        Previously, the second search operation to retrieve user entry was
        performed under the privilege of the user of their own.
      * Format of session cookie was changed.  Even if you have been used
        web interface with Sympa 6.2 or later, all users may have to login
        again after upgrade.

### From versions prior to 6.2.38

  * If you have used Oracle Database, review
    [`db_*`](/gpldoc/man/sympa.conf.5.html#database-related) parameters in
    [`sympa.conf`](../layout.md#config):

      - If you want to continue using SID (only method supported before) and
        you have not set `db_host` parameter explicitly, add a line
        ``` code
        db_host localhost
        ```
        to `sympa.conf`.
      - Or, you may choose the other methods.

    For details see
    [the instruction](../install/setup-database-oracle.md#general-instruction).

### From versions prior to 6.2.34

  * [`host`](/gpldoc/man/list_config.5.html#host) list parameter was deprecated. If you have used this parameter:

      1. Create new domain with the same name as value of `host` list parameter (Note:
         replace [``$SYSCONFDIR``](../layout.md#sysconfdir),
         [``$EXPLDIR``](../layout.md#expldir) and ``<host>`` below):
         ``` bash
         # mkdir -m 755 $SYSCONFDIR/<host>
         # touch $SYSCONFDIR/<host>/robot.conf
         # chown -R sympa:sympa $SYSCONFDIR/<host>
         # mkdir -m 750 $EXPLDIR/<host>
         # chown sympa:sympa $EXPLDIR/<host>
         ```
         And rename list `listname@domain` to `listname@host`, using web interface or
         [`sympa.pl`](/gpldoc/man/sympa.pl.1.html) command line tool.

      2. Also, you may have to check [`list_aliases.tt2`](/gpldoc/man/list_aliases.tt2.5.html) and may have to
         regenerate alias file after upgrading as:
         ``` bash
         # sympa.pl --make_alias_file --robot <mail domain>
         # sympa_newaliases.pl --domain <mail domain>
         ```
      Note that you are recommended to back up database and older alias files in advance.

  * If you managed multiple domains and used web interface,
    [`wwsympa_url`](/gpldoc/man/sympa_config.5.html#wwsympa_url) parameter in each
    [`robot.conf`](/gpldoc/man/sympa.conf.5.html) file is now mandatory.
    Though it will be automatically added during upgrading process, if you used HTTPS protocol,
    you may have to edit value of `wwsympa_url` parameter in each `robot.conf` file.

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
    [`static_content_path`](/gpldoc/man/sympa.conf.5.html#static_content_path)
    parameter in [``sympa.conf``](../layout.md#config), you might want to
    specify [`css_path`](/gpldoc/man/sympa.conf.5.html#css_path) (if you have not
    specified it) and [`pictures_path`](/gpldoc/man/sympa.conf.5.html#pictures_path)
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
    [`static_content_url`](/gpldoc/man/sympa.conf.5.html#static_content_url)
    parameter in [``sympa.conf``](../layout.md#config), you might want to
    specify [`css_url`](/gpldoc/man/sympa.conf.5.html#css_url) (if you have not
    specified it) and [`pictures_url`](/gpldoc/man/sympa.conf.5.html#pictures_url)
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

       - As some Sympa binaries have been renamed, your sympa init script have
         to be updated. Please make sure your configure options have been
         correctly set, so that this script ends up in the correct location.
         See
         "[Run configure script](../install/install-sympa-distribution-source.md#run-configure-script)"
         for details.

       - Options `--with-sendmail_aliases` and `--with-virtual_aliases` were
         deprecated.  Use --with-aliases_file instead.
         Option `--with-postmap_arg` was removed.

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
     [upgrade_bulk_spool.pl](/gpldoc/man/upgrade_bulk_spool.1.html) and
     [upgrade_send_spool.pl](/gpldoc/man/upgrade_send_spool.1.html).

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
    "`sympa.conf.upgrade.`_number_".
  - Any customized "**`web_tt2`**" directory (either in
    [``$SYSCONFDIR``](../layout.md#sysconfdir)`/` or in
    [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`_robot_`/`)
    will have been renamed to
    "`web_tt2.upgrade.`_number_".

    Indeed, as most of the Sympa templates have been changed for the new skin,
    your customizations could not be compliant to the new web interface. You
    should see how to reintroduce them to the new templates.

Additionally, you may have to fix up configuration manually:

  - New parameters:

    [`process_archive`](/gpldoc/man/list_config.html#process_archive) controls
    archiving.
    The default is "`off`": To enable archiving, it must be set to "`on`"
    explicitly (in `sympa.conf`, `robot.conf` or each list `config` file).

    If you have used `virtualwrapper`, it was replaced with new programs
    `sympa_newaliases.pl` and its wrapper: You may have to add new parameters
    [`aliases_program`](/gpldoc/man/sympa.conf.5.html#aliases_program) and
    optionally [`aliases_db_type`](/gpldoc/man/sympa.conf.5.html#aliases_db_type)
    to `sympa.conf`.

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

