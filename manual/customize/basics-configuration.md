---
title: 'Configuration hierarchy'
prev: basics-roles.md
up: ../customize.md#customization-basics
next: basics-list-config.md
---

Configuration hierarchy
=======================

Configuration of Sympa may be classified into multiple scopes:
"_List_", "_mail domain_", "_site_" and distribution default.

----
Note:

  * In earlier documentations, a term "**robot**" was chosen to refer to
    "mail domain" scope.  If you see this word in the other place of this
    document, please replace it with "mail domain".

----

Configuration files
-------------------

### Location

To find a configuration file, Sympa searches following four directories in
order, i.e. from narrower to wider scopes:

  - [``$EXPLDIR``](../layout.md#expldir)`/`*list path*`/`
    --- List scope (see the note below)
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`*mail domain name*`/`
    --- Mail domain scope
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`
    --- Site scope
  - [``$DEFAULTDIR``](../layout.md#defaultdir)`/`
    --- Distribution default

Once desired file is found in a directory, Sympa no longer does search
subsequent directories.  By this hierarchical system, configuration applied
to wider scopes can be customized for particular domains and/or lists.

For example, given following [`edit_list.conf`](/gpldoc/man/edit_list.conf.5.html)
configuration files placed:

  - `$EXPLDIR/mail.example.org/list1/edit_list.conf`
  - `$SYSCONFDIR/mail.example.org/edit_list.conf`
  - `$SYSCONFDIR/edit_list.conf`
  - `$DEFAULTDIR/edit_list.conf`

On a list `list@mail.example.org`, the first file will be used.
However, on the other lists of `mail.example.org` (e.g.
`other@mail.example.org`), the second file will be used.
On the lists of the other domain (e.g. `list@other.example.org`), the third
file will be used.
The last file, distribution default, will never be used in this example,
because the other file supersedes it.

----
Note:

  * On list scope, canonical path of configuration is:
    > `$EXPLDIR/`*mail domain name*`/`*list name*`/`

    However, by historical reason, the lists of primary domain (the mail
    domain defined by [`domain`](/gpldoc/man/sympa.conf.5.html#domain) parameter in
    [``sympa.conf``](../layout.md#config)) also allows the path in a style:
    > `$EXPLDIR/`*list name*`/`.

    The former style is recommended.  The latter style may be deprecated
    by the future version of Sympa.

----

Exceptions:

  - Some configuration files are not available in "list" and/or "mail domain"
    scopes.  For example:

      - [`auth.conf`](/gpldoc/man/auth.conf.5.html) is available only in
        "mail domain" or "site" scope;
      - [`charset.conf`](/gpldoc/man/charset.conf.5.html) is available only in "site"
        scope.

    See also
    [documentation on each file](/gpldoc/man/sympa_toc.1.html#configuration-files).

  - Most of templates are classified further by subdirectories: See
    "[Templates](basics-templates.md)".  Task model files are alike: See
    "[Tasks](basics-tasks.md)".

### Changing configuration files

When you customize configuration files, *don't* edit files under
[``$DEFAULTDIR``](../layout.md#defaultdir) directly. Changes on the files
under this directory will be overwritten by new version during upgrade
process.

Instead, you should copy files under $DEFAULTDIR directory into the directory
of appropriate scope (see previous section), then edit these copies.

Configuration parameters
------------------------

Main configuration files have different names by each scope:

  - [``$EXPLDIR``](../layout.md#expldir)`/`*list path*`/config`
    --- List configuration
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`*mail domain name*`/robot.conf`
    --- Mail domain configuration
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/sympa.conf`
    --- Site configuration
  - Distribution defaults of parameters are defined in the source code of
    Sympa.

These files will not override the others.  Instead, several parameters in
these files override parameters in wider scopes by each.  See documentation on
parameters in [`sympa.conf`, `robot.conf`](/gpldoc/man/sympa.conf.5.html) and
[`config`](/gpldoc/man/list_config.5.html) files.

