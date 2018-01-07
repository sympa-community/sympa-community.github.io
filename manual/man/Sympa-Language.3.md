---
title: 'Sympa::Language(3)'
---

# NAME

Sympa::Language - Handling languages and locales

# SYNOPSIS

    use Sympa::Language;
    my $language = Sympa::Language->instance;
    $language->set_lang('zh-TW', 'zh', 'en');
    
    print $language->gettext('Lorem ipsum dolor sit amet.');

# DESCRIPTION

This package provides interfaces for i18n (internationalization) of Sympa.

The language tags are used to determine each language.
A language tag consists of one or more subtags: language, script, region and
variant.  Below are some examples.

- `ar` - Arabic language
- `ain` - Ainu language
- `pt-BR` - Portuguese language in Brazil
- `be-Latn` - Belarusian language in Latin script
- `ca-ES-valencia` - Valencian variant of Catalan

Other two sorts of identifiers are derived from language tags:
gettext locales and POSIX locales.

The gettext locales determine each translation catalog.
It consists of one to three parts: language, territory and modifier.
For example, their equivalents of language tags above are `ar`, `ain`,
`pt_BR`, `be@latin` and `ca_ES@valencia`, respectively.

The POSIX locales determine each _locale_.  They have similar forms to
gettext locales and are used by this package internally.

## Functions

### Manipulating language tags

- canonic\_lang ( $lang )

    _Function_.
    Canonicalizes language tag according to RFC 5646 (BCP 47) and returns it.

    Parameter:

    - $lang

        Language tag or similar thing.
        Old style "locale" by Sympa (see also ["Compatibility"](#compatibility)) will also be
        accepted.

    Returns:

    Canonicalized language tag.
    In array context, returns an array
    `(_language_, _script_, _region_, _variant_)`.
    For malformed inputs, returns `undef` or empty array.

    See ["CAVEATS"](#caveats) about details on format.

- implicated\_langs ( $lang, ... )

    _Function_.
    Gets a list of each language $lang itself and its "super" languages.
    For example:
    If `'tyv-Latn-MN'` is given, this function returns
    `('tyv-Latn-MN', 'tyv-Latn', 'tyv')`.

    Parameters:

    - $lang, ...

        Language tags or similar things.
        They will be canonicalized by ["canonic\_lang"](#canonic_lang)()
        and malformed inputs will be ignored.

    Returns:

    A list of implicated languages, if any.
    If no $lang arguments were given, this function will die.

- lang2locale ( $lang )

    _Function_, _internal use_.
    Convert language tag to gettext locale name
    (see also ["Native language support (NLS)"](#native-language-support-nls)).
    This function may be useful if you want to know internal information such as
    name of catalog file.

    Parameter:

    - $lang

        Language tag or similar thing.

    Returns:

    The gettext locale name.
    For malformed inputs returns `undef`.

- negotiate\_lang ( $string, $lang, ... )

    _Function_.
    Get the best language according to the content of `Accept-Language:` HTTP
    request header field.

    Parameters:

    - $string

        Content of the header.  If it is false value, `'*'` is assumed.

    - $lang, ...

        Acceptable languages.

    Returns:

    The best language or, if negotiation failed, `undef`.

### Compatibility

As of Sympa 6.2b, language tags are used to specify languages along with
locales.  Earlier releases used POSIX locale names.

These functions are used to migrate data structures and configurations of
earlier versions.

- lang2oldlocale ( $lang )

    _Function_.
    Convert language tag to old-style "locale".

    Parameter:

    - $lang

        Language tag or similar thing.

    Returns:

    Old-style "locale".
    If corresponding locale could not be determined, returns `undef`.

    Note:
    In earlier releases this function was named Lang2Locale()
    (don't confuse with ["lang2locale"](#lang2locale)()).

## Methods

- instance ( )

    _Constructor_.
    Gets the singleton instance of [Sympa::Language](./Sympa-Language.3.md) class.

### Getting/setting language context

- push\_lang ( \[ $lang, ... \] )

    _Instance method_.
    Set current language by ["set\_lang"](#set_lang)() keeping the previous one;
    it can be restored with ["pop\_lang"](#pop_lang)().

    Parameter:

    - $lang, ...

        Language tags or similar things.

    Returns:

    Always `1`.

- pop\_lang

    _Instance method_.
    Restores previous language.

    Parameters:

    None.

    Returns:

    Always `1`.

- set\_lang ( \[ $lang, ... \] )

    _Instance method_.
    Sets current language along with translation catalog,
    and POSIX locale if possible.

    Parameter:

    - $lang, ...

        Language tags or similar things.
        Old style "locale" by Sympa (see also ["Compatibility"](#compatibility)) will also be
        accepted.
        If multiple tags are specified, this function tries each of them in order.

        Note that `'en'` will always succeed.  Thus, putting it at the end of
        argument list may be useful.

    Returns:

    Canonic language tag actually set or, if no usable catalogs were found,
    `undef`.  If no arguments are given, do nothing and returns `undef`.

    Note that the language actually set may not be identical to the parameter
    $lang, even when latter has been canonicalized.

    The language tag `'en'` is special:
    It is used to set `'C'` locale and will succeed always.

    Note:
    This function of Sympa 6.2a or earlier returned old style "locale" names.

- native\_name ( )

    _Instance method_.
    Get the name of the language, i.e. the one defined in the catalog.

    Parameters:

    None.

    Returns:

    Name of the language in native notation.
    If it was not found, returns an empty string `''`.

    Note:
    The name is the content of `Language-Team:` field in the header of catalog.

- get\_lang ()

    _Instance method_.
    Get current language tag.

    Parameters:

    None.

    Returns:

    Current language.
    If it is not known, returns default language tag.

### Native language support (NLS)

- dgettext ( $domain, $msgid )

    _Instance method_.
    Returns the translation of given string using NLS catalog in domain $domain.
    Note that ["set\_lang"](#set_lang)() must be called in advance.

    Parameter:

    - $domain

        gettext domain.

    - $msgid

        gettext message ID.

    Returns:

    Translated string or, if it wasn't found, original string.

- gettext ( $msgid )

    _Instance method_.
    Returns the translation of given string using current NLS catalog.
    Note that ["set\_lang"](#set_lang)() must be called in advance.

    Parameter:

    - $msgid

        gettext message ID.

    Returns:

    Translated string or, if it wasn't found, original string.

    If special argument `'_language_'` is given,
    returns the name of language in native form (See [native\_name](https://metacpan.org/pod/native_name)()).
    For argument `''` returns empty string.

- gettext\_sprintf ( $format, $args, ... )

    _Instance method_.
    Internationalized [sprintf](https://metacpan.org/pod/sprintf)().
    At first, translates $format argument using ["gettext"](#gettext)().
    Then returns formatted string by remainder of arguments.

    This is equivalent to `sprintf( gettext($format), $args, ... )`
    with appropriate POSIX locale if possible.

    Parameters:

    - $format

        Format string.
        See also ["sprintf" in perlfunc](https://metacpan.org/pod/perlfunc#sprintf).

    - $args, ...

        Arguments fed to sprintf().

    Returns:

    Translated and formatted string.

- gettext\_strftime ( $format, $args, ... )

    _Instance method_.
    Internationalized [strftime](https://metacpan.org/pod/POSIX#strftime)().
    At first, translates $format argument using ["gettext"](#gettext)().
    Then returns formatted date/time by remainder of arguments.

    If appropriate POSIX locale is not available, parts of result (names of days,
    months etc.) will be taken from the catalog.

    Parameters:

    - $format

        Format string.
        See also ["strftime" in POSIX](https://metacpan.org/pod/POSIX#strftime).

    - $args, ...

        Arguments fed to POSIX::strftime().

    Returns:

    Translated and formatted string.

- maketext ( $textdomain, $template, $args, ... )

    _Instance method_.
    At first, translates $template argument using ["gettext"](#gettext)().
    Then replaces placeholders (`%1`, `%2`, ...) in template with arguments.

    Numeric arguments will be formatted using appropriate locale, if any:
    Typically, the decimal point specific to each locale may be used.

    Parameters:

    - $textdomain

        NLS domain to be used for searching catalogs.

    - $template

        Template string which may include placeholders.

    - $args, ...

        Arguments corresponding to placeholders.

    Returns:

    Translated and replaced string.

**Note**:

Calls of ["gettext"](#gettext)(), ["gettext\_sprintf"](#gettext_sprintf)() and ["gettext\_strftime"](#gettext_strftime)() are 
extracted during packaging process and are added to translation catalog.

# CAVEATS

- We impose some restrictions and modifications to the format described in
BCP 47:
language extension subtags won't be supported;
if script and variant subtags co-exist, latter will be ignored;
the first one of multiple variant subtags will be used;
each variant subtag may be longer than eight characters;
extension subtags are not supported.
- Since catalogs for `zh`, `zh-Hans` or `zh-Hant` may not be provided,
["set\_lang"](#set_lang)() will choose approximate catalogs for these tags.

# SEE ALSO

RFC 5646 _Tags for Identifying Languages_.
[http://tools.ietf.org/html/rfc5646](http://tools.ietf.org/html/rfc5646).

_Translating Sympa_.
[https://translate.sympa.org/pages/help](https://translate.sympa.org/pages/help).

# HISTORY

[Language](https://metacpan.org/pod/Language) module supporting multiple languages by single installation
and using NLS catalog in msgcat format appeared on Sympa 3.0a.

Sympa 4.2b.3 adopted gettext portable object (PO) catalog and POSIX locale.

On Sympa 6.2, rewritten module [Sympa::Language](./Sympa-Language.3.md) adopted BCP 47 language tag
to determine language context, and installing POSIX locale became optional.
