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

### Custom filter directives

Sympa also implements some custom
[filter directives](http://www.template-toolkit.org/docs/manual/Filters.html).  For
example, with the `loc` filter directive, the text:
``` code
[%|loc%]A text[%END%]
```
will be replaced with localized phrase of `A text` using translation catalogs
(see also "[Internationalization](basics-i18n.md)").

See ["Filters" in Sympa::Template(3)](/gpldoc/man/Sympa-Template.3.html#filters)
on custom filter directives provided by Sympa.

### Attached message in mail template

Some mail templates may include another mail messages as attachments, i.e.
the MIME parts with `message/rfc822` type.  For example, below is an excerpt
from `mail_tt2/digest.tt2` for digest (MIME format) messages:
``` code
...
Content-Type: multipart/digest; boundary="[% boundary2 %]"
Mime-Version: 1.0

This is a multi-part message in MIME format...

[% FOREACH m = msg_list -%]
--[% boundary2 %]
Content-Type: message/rfc822
Content-Disposition: inline
X-Sympa-Attach: yes

[%# message inserted here #%]

[% END %]
--[% boundary2 %]--

...
```
If a part with `X-Sympa-Attach` pseudo-header field appears, its body will
be replaced with a message, discarding original body.  This way, messages may
be attached with their contents not being altered.

### Line wrapping in mail templates

By default, text body of mail template (except attached part described in above)
is wrapped.  `X-Sympa-NoWrap` pseudo-header field prevents line wrapping.

Location
--------

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
~~[mail command](../mail-commands.md)~~.
On the other hand, `web_tt2/modindex.tt2` file is used to generate the page
to list messages waiting for moderation.

See also
"[Location](basics-configuration.md#location)" in "Configuration hierarchy"
about location of template files and how to customize them.

Mail and web templates may also be customized according to language contexts:
See
"[Defining language-specific templates](basics-i18n.md#defining-language-specific-templates)"
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

[`mhonarc_rc.tt2`](/gpldoc/man/mhonarc_rc.tt2.5.html)
is used to generate message archives available on web interface.
It may be customized by each mail domain and/or each list: See
"[Location](basics-configuration.md#location) in "Configuration hierarchy".

[Custom tag delimiters](#custom-tag-delimiters) are used in this file.

> **Note**
>
>   * On Sympa 6.2.60 or earlier, the name of this file was
>     `mhonarc-ressources.tt2` (take care of spelling), and the variable tag
>     delimiters were used.

### Template file for mail aliases

~~[`list_aliases.tt2`](../man/list_aliases.tt2)~~ is used by alias manager of
Sympa to generate mail aliases so that message delivery agent (MDA) will
be able to recognize list addresses.  It may be customized by each mail
domain: See
"[Location](basics-configuration.md#location) in "Configuration hierarchy".

### Other uses

  - Value of [`custom_subject`](/gpldoc/man/sympa_config.5.html#custom_subject)
    list configuration parameter may include template syntax:
    `[% list.name %]` and `[% list.sequence %]` variables may be used.

  - On the lists
    [Message personalization](/gpldoc/man/sympa_config.5.html#merge_feature) enabled,
    Sympa treats the bodies of incoming messages as templates, and delivers
    messages customized by each member.  See
    "~~[Message personalization](../customize/web-mailer.md#message-personalization)~~"
    for details.

(Work in progress)

Custom tag delimiters
---------------------

In some cases, a template have to generate another template.  To avoid
breaking template syntax,
[custom tag delimiters](http://www.template-toolkit.org/docs/manual/Directives.html#section_TAGS) are useful.

Example:

To include a setting in the list creation template:
```
custom_subject [% list.name %]:[% list.sequence %]
```
use custom tag delimiters in `config.tt2` as below:
```
...

[% TAGS <+ +> %]
custom_subject [% list.name %]:[% list.sequence %]
<+ TAGS [% %] +>

...
```
By the first `TAGS` directive, special meaning of "`[% ... %]`" is dropped.
Then by the second `TAGS`, its meaning is restored.

Template plugins
----------------

You can extend syntax of template by writing Perl module.  See
"[Template plugins](../customize/template-plugins.md)".

