---
title: 'Sympa Internationalization'
prev: basics-tasks.md
up: ../customize.md#customization-basics
next: basics-families.md
---

Sympa Internationalization
==========================

(Work in progress)

Sympa internationalization
--------------------------

(Work in progress)

### List internationalization

(Work in progress)

### User internationalization

(Work in progress)

Language tag
------------

Sympa uses "language tag" to determine context of language and locale for users and lists. The language tag consists of one or more subtags: language, script, region and variant. Below are some examples.

| Language tag     | Description                        |
|------------------|------------------------------------|
| `ar`             | Arabic language (common)           |
| `pt-BR`          | Portuguese language in Brazil      |
| `sr-Latn`        | Serbian language with Latin script |
| `ca-ES-valencia` | Catalan spoken in Valencia         |
| `ryu`            | Utinaaguti (around Okinawa island) |

----
Note:

  * Until Sympa 6.1.x, language contexts are based on POSIX locale. Its naming rule was not standardized enough, and also had difficulties to handle particular languages. Language tag is roughly based on [BCP 47](https://tools.ietf.org/html/bcp47) published by IETF. As of Sympa 6.2, POSIX locale names in old style are still supported but they are converted to language tags internally.

----

Translation catalogs and templates
----------------------------------

Sympa is designed to allow easy internationalization of its user interface (service mail messages and web interface). All translations for one language are gathered in a single .po file that can be manipulated by standard [GNU gettext tools](https://www.gnu.org/software/gettext/#introduction).

[Instructions for translating Sympa](https://translate.sympa.org/pages/help) are maintained.

----
Note:

  * The "gettext locale name" is used for naming of `.po` file. Sympa maps language tags to gettext locale names and vice versa. Equivalents of language tags in example above are `ar`, `pt_BR`, `sr@latin`, `ca_ES@valencia` and `ryu`, respectively. If you don't know what name to use for your language, please ask Sympa authors.

----

Sympa templates refer to translatable strings using the `loc` TT2 filter.

Examples:

``` code
[%|loc%]User Email[% END %]
```

``` code
[%|loc(list.name,user.email)%]You have subscribed to list %1 with email address %2[% END %]
```

Sympa had previously been translated into 15 languages more or less completely. We have automatically extracted the translatable strings from previous templates but this process is awkward and is only seen as a bootstrap for translators. Therefore Sympa distribution will not include previous translations until a skilled translator has reviewed and updated the corresponding .po file.

Defining language-specific templates
------------------------------------

The default Sympa templates are language independant, refering to catalogue entries for translations. When customizing either web or mail templates, you can define different templates for different languages. The template should be located in a language subdirectory of `web_tt2` or `mail_tt2` with the language tag.

Example :

``` code
/web_tt2/home.tt2
/web_tt2/de/home.tt2
/web_tt2/en-US/home.tt2
/web_tt2/fr/home.tt2
```

This mechanism also applies to `comment.tt2` files used by create list templates.

Web templates can also make use of the `lang` variable to make templates multi-lingual:

Example :

``` code
[% IF lang == 'fr' %]
Personnalisation
[% ELSE %]
Customization
[% END %]
```

`lang` variable is also defined for JavaScript.

----
Note:

  * Until Sympa 6.1.x, `locale` variable was used.

----

### Overriding cascading style sheets by language context

`web_tt2/css.tt2` for specific languages will override any portion of main css, not fully replacing it. So they may be used for customization for particular language.

For example, default css.tt2 specifies the font families covering Western scripts (Cyrillic, Latin, ...). East Asian users may prefer to consistent font family supporting Western along with Eastern scripts (hangeul, hanzi, ...). Sympa 6.1.18 and later include `css.tt2` specific to these languages (`web_tt2/ja/css.tt2`, `web_tt2/ko/css.tt2`, `web_tt2/zh-CN/css.tt2` and `web_tt2/zh-TW/css.tt2`).

For another example, users using languages with right-to-left scripts (Arabic, Hebrew, ...) might wish web page layout to be fit in direction of texts. It may be possible to override attributes for alignment by `css.tt2` for each language (`web_tt2/ar/css.tt2` etc.).

Translating titles of topics, scenarios and description of list creation templates
----------------------------------------------------------------------------------

List topics are defined in a [`topics.conf`](/gpldoc/man/topics.conf.5.html) file.
In this file, each entry can be given a title in different languages.  See "~~[List topics](../customize/list-topics.md)~~".

Scenarios and comments in list creation templates can have titles by multiple languages. See "[Content of scenario file](basics-scenarios.md#content-of-scenario-file)" and "[comment.tt2](../admin/list-creation.md#commenttt2)".

Handling of character sets
--------------------------

Until version 5.3, Sympa web pages were using in each language's character set (iso-8859-1 for French, utf-8 for Japanese, ...) whereas every web page now uses utf-8. Sympa also expects every file to be UTF-8 including : configursation files, templates, authorization scenarios, .po files.

Note that the shared documents (see "~~[Shared document repository](../customize/shared-repository.md)~~") filenames are Q-encoded to make their storage encoding neutral. This encoding is transparent for end users.

### Support for legacy character sets

Sympa fully supports Unicode. By default, all messages sent by Sympa will be encoded by `utf-8` character set. However, in some language environments legacy character sets are preferred, for example `iso-2022-jp` in Japanese language. Messages sent by Sympa may be encoded using legacy character sets spesific to each language. See [`legacy_character_support_feature`](/gpldoc/man/sympa.conf.5.html#legacy_character_support_feature) parameter.

Note that this setting does not affect messages sent by users but only the messages mailing list robot will send.
