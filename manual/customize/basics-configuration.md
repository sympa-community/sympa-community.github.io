---
title: 'Configuration hierarchy'
prev: basics-roles.md
up: ../customize.md#customization-basics
next: basics-templates.md
---

Configuration hierarchy
=======================

Configuration files
-------------------

Configuration of Sympa may be classified in multiple contexts:
"_List_", "_mail domain_", "_site_" and distribution default.
To find a configuration, Sympa searches following four directories in order,
i.e. from lower to higher level of contexts:

  - [``$EXPLDIR``](../layout.md#expldir)`/`*list path*`/`
    --- List context
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`*mail domain name*`/`
    --- Mail domain context
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`
    --- Site context
  - [``$DEFAULTDIR``](../layout.md#defaultdir)`/`
    --- Distribution default

And once it is found in a directory, Sympa no longer does search subsequent
directories.  By this hierarchical system, global configuration can be
customized only on particular domains and/or on particular lists.

----
Notes:

  * In earlier documentations, a term "**robot**" was chosen to refer to
    "mail domain" context.  If you see this word in other place of this
    document, please replace it with "mail domain".

  * About *list path*:

    On list context, canonical path of configuration is:
    > `$EXPLDIR/`*mail domain name*`/`*list name*`/`

    However, by historical reason, the list of primary domain (the mail
    domain defined in [``sympa.conf``](../layout.md#config)) also allows the
    path in this style:
    > `$EXPLDIR/`*list name*`/`.

    The former style is recommended.  The latter style may be deprecated
    by the future version of Sympa.

----

For example, given following [`edit_list.conf`](../man/edit_list.conf.5.md)
files placed:

  - `$EXPLDIR/mail.domain.org/list1/edit_list.conf`
  - `$SYSCONFDIR/mail.domain.org/edit_list.conf`
  - `$SYSCONFDIR/edit_list.conf`
  - `$DEFAULTDIR/edit_list.conf`

On the list `list1@mail.domain.org`, the first file will be used.
However, on the other lists of `mail.domain.org` (e.g.
`list2@mail.domain.org`), the second file will be used.
On the lists of the other domain (`list1@other.dom.ain`), the third file
will be used. The last file, distribution default, will not be used because
the other file supersedes it.

Exceptions:

  - Some configuration files are not available on "list" or "mail domain"
    level.  For example:
      - [`auth.conf`](../man/auth.conf.5.md) is available only on
        "mail domain" or "site" context;
      - [`charset.conf`](../man/charset.conf.5.md) is available only on "site"
        context.
    See also
    [documentation on each file](../man/sympa_toc.1.md#configuration-files).

  - Most of templates are categorized further by subdirectories (types and
    languages): See "~~[Templates](basics-templates.md)~~".
    Tasks are alike: See "[Tasks](basics-tasks.md)".

### How to customize configuration files

When you customize configuration files, *don't* edit files under
[``$DEFAULTDIR``](../layout.md#defaultdir) directly. Changes on the files
under this directory will be overwritten by new version during upgrade
process.

Instead, you should copy files under $DEFAULTDIR into the directory
of appropriate context (see previous section), then edit these copies.

Configuration parameters
------------------------

Main configuration files have different names by each context:

  - [``$EXPLDIR``](../layout.md#expldir)`/`*list path*`/config`
    --- List configuration
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`*mail domain name*`/robot.conf`
    --- Domain configuration
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`sympa.conf`
    --- Site configuration
  - Distribution defaults of parameters are defined in the source code of
    Sympa.

These files will not override the others. However, several parameters in these
files override setting in higher levels by each.  See documentation on
parameters in [`sympa.conf`, `robot.conf`](../man/sympa.conf.5.md) and
[`config`](../man/list_config.5.md) files.

