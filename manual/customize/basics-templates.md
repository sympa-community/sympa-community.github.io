---
title: 'Templates'
prev: basics-configuration.md
up: ../customize.md#customization-basics
next: basics-scenarios.md
---

Templates
=========

Sympa provides several templates to generate variable texts in system
messages, web interface and list configuration.

Syntax
------

Sympa adopted [Template Toolkit](http://www.template-toolkit.org/) as of
version 4.2.  Below is an example of template file content:
``` code
[%# welcome.tt2 ~%]
From: [% fromlist %]
Subject: [%|qencode%][%|loc(list.name)%]Welcome to list %1[%END%][%END%]

[%|loc(list.name,list.host)%]Welcome to list %1@%2[%END%]
[%|loc(user.email)%]Your subscription email is %1[%END%]

[% TRY %]
[% INSERT "info" %]
[% CATCH %]
[% END %]

[%|loc%]The list homepage:[%END%] [% 'info' | url_abs([list.name]) %]
```

Variable items and control directives are enclosed in the delimiters
`[%` ... `%]`, and will be replaced with appropriate texts at run time.  See
the [User Manual](http://www.template-toolkit.org/docs/manual/) to know
details about syntax.

### Custom filters

Sympa also implements some custom
[filters](http://www.template-toolkit.org/docs/manual/Filters.html).  For
example, with the `loc` filter, the text:
``` code
[%|loc%]A text[%END%]
```
will be replaced with localized phrase of `A text` using translation catalogs
(see also "[Internationalization](basics-i18n.md)").

See ["Filters" in Sympa::Template(3)](../man/Sympa-Template.3.md#filters)
on custom filters proveded by Sympa.

Files
-----

### Mail and web template files

Mail template files are used to generate system messages sent by Sympa.
To find a mail template file, Sympa searches following directories in order:
  - [``$EXPLDIR``](../layout.md#expldir)`/`*list path*`/mail_tt2/`
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`*mail domain name*`/mail_tt2/`
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/mail_tt2/`
  - [``$DEFAULTDIR``](../layout.md#defaultdir)`/mail_tt2/`
    --- distribution default

Web template files are used to generate content of pages shown by web
interface.  To find a web template file, Sympa searches following directories
in order:
  - [``$EXPLDIR``](../layout.md#expldir)`/`*list path*`/web_tt2/`
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`*mail domain name*`/web_tt2/`
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/web_tt2/`
  - [``$DEFAULTDIR``](../layout.md#defaultdir)`/web_tt2/`
    --- distribution default

For example, `mail_tt2/modindex.tt2` file is used to send content of
moderation spool as the result of `MODINDEX`
~~[mail command](/manual/mail-commands.md)~~.
On the other hand, `web_tt2/modindex.tt2` file is used to generate the page
to list messages waiting for moderation.

See also
"[Location](basics-configuration.md#location)" in "Configuration hierarchy"
about location of template files and how to customize them.

Mail and web templates may also be customized according to language contexts:
See
"[Translation catalogs and templates](basics-i18n.md#translation-catalogs-and-templates)"
in "Internationalization".

### List creation templates

List creation templates may contain `config.tt2` and `comment.tt2` files.
`config.tt2` will be used to generate [`config`](../man/list_config.tt2)
file during list creation.  See
"[Typical list profile](../admin/list-creation.md#typical-list-profile)" in
"List creation".

Family feature may generate more list configuration files using template.
See "[Using family](basics-families.md#using-family) for details.

To know how to generate template files using template, see also "[Custom tag delimiters](#custom-tag-delimiters)".

### Archive template

[`mhonarc-ressources.tt2`](../man/mhonarc-ressources.tt2.5.md) (take care of
spelling) is used to generate message archives available on web interface.
It may be customized by each mail domain and/or each list: See
"[Location](basics-configuration.md#location) in "Configuration hierarchy".

[Custom tag delimiters](#custom-tag-delimiters) are used in this file.

### Template file for mail aliases

[`list_aliases.tt2`](../man/list_aliases.tt2) is used by alias manager of
Sympa to generate mail aliases so that message delivery agent (MDA) will
be able to recognize list addresses.  It may be customized by each mail
domain: See
"[Location](basics-configuration.md#location) in "Configuration hierarchy".

### Use of template syntax in parameters

Template syntax may be used in following list configuration parameters:

  - [`custom_subject`](../man/list_config.5.md#custom_subject)

    `[% list.name %]` and `[% list.sequence %]` variables may be used.

(Work in progress)

Custom tag delimiters
---------------------

In some cases, a template have to generate another template.  To avoid
breaking template syntax,
[custom tag delimiters](http://www.template-toolkit.org/docs/manual/Directives.html#section_TAGS) are useful.

(Work in progress)

Template plugins
----------------

You can extend syntax of template by writing Perl code.  See
"[Template plugins](../customize/template-plugins.md)".

