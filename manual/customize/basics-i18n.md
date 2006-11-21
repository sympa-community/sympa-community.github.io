Sympa Internationalization
==========================

Catalogs and templates
----------------------

Sympa is designed to allow easy internationalization of its user interface (service mail messages and web interface). All translations for one language are gathered in a single PO file that can be manipulated by standard [GNU gettext tools](http://www.gnu.org/software/gettext/#TOCintroduction).

Documentation and ressources about software translations : [http://translate.sourceforge.net/doc/](http://translate.sourceforge.net/doc/)

Sympa previously (until Sympa 4.1.x) used XPG4 messages catalogue format. Web and mail templates were language specific. The new organization both provide a unique file to work on for translators and a standard format supported by many software. Sympa templates refer to translatable strings using the `loc` TT2 filter.

Examples :

``` code
[%|loc%]User Email[%END%]

[%|loc(list.name,user.email)%]You have subscribed to list %1 with email address %2[%END%]
```

Sympa had previously been translated into 15 languages more or less completely. We have automatically extracted the translatable strings from previous templates but this process is awkward and is only seen as a bootstrap for translators. Therefore Sympa distribution will not include previous translations until a skilled translator has reviewed and updated the corresponding PO file.

Translating Sympa GUI in your language
--------------------------------------

Instructions for translating Sympa are maintained on Sympa web site : [http://www.sympa.org/howtotranslate.html](http://www.sympa.org/howtotranslate.html)

Defining language-specific templates
------------------------------------

The default Sympa templates are language independant, refering to catalogue entries for translations. When customizing either web or mail templates, you can define different templates for different languages. The template should be located in a ll\_CC subdirectory of `web_tt2` or `mail_tt2` with the language code.

Example :

``` code
/web_tt2/home.tt2
/web_tt2/de_DE/home.tt2
/web_tt2/fr_FR/home.tt2
```

This mecanism also applies to `comment.tt2` files used by create list templates.

Web templates can also make use of the `locale` variable to make templates multi-lingual :

Example :

``` code
[% IF locale == 'fr_FR' %]
Personnalisation
[% ELSE %]
Customization
[% END %]
```

Translating topics titles
-------------------------

Topics are defined in a `topics.conf` file. In this file, each entry can be given a title in different languages, see [17.5][(]customize/basics-templates.md#topics), page [![\[\*\]](crossref.png)][(]customize/basics-templates.md#topics).

Handling of encodings
---------------------

Until version 5.3, Sympa web pages were encoded in each language's encoding (iso-8859-1 for French, utf-8 for Japanese,...) whereas every web page is now encoded in utf-8. Thanks to the `Encode` Perl module, Sympa can now juggle with the filesystem encoding, each message catalog's encoding and its web encoding (utf-8).

If your operating system uses a character encoding different from utf-8, then you should declare it using the `filesystem_encoding` sympa.conf parameter (see [![\[\*\]](crossref.png)](#filesystem-encoding), page [![\[\*\]](crossref.png)](#filesystem-encoding)). It is required to do so because Sympa has no way to find out what encoding is used for its configuration files. Once this encoding is known, every template or configuration parameter can be read properly for the web and also saved properly when edited from the web interface.

Note that the shared documents (see[23][(]customize/shared-repository.md#shared), page [![\[\*\]](crossref.png)][(]customize/shared-repository.md#shared)) filenames are Q-encoded to make their storage encoding neutral. This encoding is transparent for the end-users.

