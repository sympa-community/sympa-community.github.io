Sympa Administration Manual **[DRAFT]**
===========================

Version 6.2.22+

Table of contents
-----------------

  - [Audiences and scope](scope.md)
  - [Requirements](requirements.md)
  - [Installing Sympa](install.md)
  - [Upgrading Sympa](upgrade.md)
  - [Administering Sympa](admin.md)
  - [Customizing Sympa](customize.md)

----
  - [References](man/sympa_toc.1.md)
  - [Directory layout](layout.md)

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

