Sympa Administration Manual
===========================

Version 6.2.50

Table of contents
-----------------

  - [Audiences and scope](scope.md)
  - [Requirements](requirements.md)
  - [Installing Sympa](install.md)

      - [Install Sympa distribution](install/install-sympa-distribution.md)
      - [Install dependent modules](install/install-dependent-modules.md)
      - [Generate initial configuration](install/generate-initial-configuration.md)
      - [Setup database](install/setup-database.md)
      - [Configure system log](install/configure-system-log.md)
      - [Configure mail server](install/configure-mail-server.md)
      - [Configure HTTP server](install/configure-http-server.md)
      - [Start mailing list service](install/start-mailing-list-service.md)
      - [Automate startup](install/automate-startup.md)

  - [Upgrading Sympa](upgrade.md)

      - [Upgrading Sympa in place](upgrade/in-place.md)
      - [Moving to another server](upgrade/move.md)
      - [Upgrading notes](upgrade/notes.md)

  - [Administering Sympa](admin.md)

      - [Managing services](admin/services.md)
      - [Command line interface](admin/cli.md)
      - [Managing lists](admin/list.md)

          - [List creation, editing and removal](admin/list-creation.md)
          - [Managing list members, owners and moderators](admin/list-members.md)

      - [Web interface for listmaster](admin/web-interface.md)

  - [Customizing Sympa](customize.md)

      - [Customization basics](customize.md#customization-basics)

          - [Message workflow](customize/basics-workflow.md)
          - [Roles and privileges](customize/basics-roles.md)
          - [Configuration hierarchy](customize/basics-configuration.md)
          - [Configuration for each list](customize/basics-list-config.md)
          - [Templates](customize/basics-templates.md)
          - [Authorization scenarios](customize/basics-scenarios.md)
          - [Tasks](customize/basics-tasks.md)
          - [Internationalization](customize/basics-i18n.md)
          - [List families](customize/basics-families.md)

      - [Customizing Sympa services](customize.md#customizing-sympa-services)
      - [Customizing web interface](customize.md#customizing-web-interface)
      - [Sympa services: Optional features](customize.md#sympa-services-optional-features)
      - [Web interface: Optional features](customize.md#web-interface-optional-features)
      - [Extending Sympa](customize.md#extending-sympa)
      - [Sympa and other systems](customize.md#sympa-and-other-systems)
      - [Other topics](customize.md#other-topics)

Appendices

  - [References](/gpldoc/man/sympa_toc.1.html)

      - [Reference manual](/gpldoc/man/sympa_toc.1.html#reference-manual)
      - [Daemons](/gpldoc/man/sympa_toc.1.html#daemons)
      - [Web interface](/gpldoc/man/sympa_toc.1.html#web-interface)
      - [Shell Interface](/gpldoc/man/sympa_toc.1.html#shell-interface)
      - [Auxiliary Programs](/gpldoc/man/sympa_toc.1.html#auxiliary-programs)
      - [Configuration files](/gpldoc/man/sympa_toc.1.html#configuration-files)
      - [Internals](/gpldoc/man/sympa_toc.1.html#internals)

  - [Directory layout](layout.md)

  - [Documentation on older versions](/gpldoc/manual-old/)

Manual convention
-----------------

  * The ``monospace font`` represents the text readable by machine:
    Name of any hosts, directories, files, commands or parameters,
    or value of any parameters.

    Example:
    > The ``pw`` command may add a user to ``/etc/passwd``.

  * Fenced block represents input and/or output on the console,
    or excerpt from any file.

    Example:
    > ```bash
    > $ echo 'Sympa!'
    > Sympa!
    > ```

    Example:
    > ```
    > domain mail.exmaple.org
    > wwsympa_url https://web.example.org/sympa
    > ```

  * ``$ `` or ``# `` prepended to the line represents shell prompt.
    ``$ `` indicates that commands may be executed by unprivileged user.
    ``# `` indicates that commands should be executed under super-user (root)
    privilege.  In latter case sudo(1) may also be used.

    Example:
    ```bash
    # make install
    ```

    is the same as
    ```bash
    $ sudo make install
    ```

  * To help readers understand, several symbols are used to stand for
    particular path names.  See "[Directory layout](layout.md)".

