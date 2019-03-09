---
title: 'Sympa::Template(3)'
---

# NAME

Sympa::Template - Template parser

# SYNOPSIS

    use Sympa::Template;
    
    $template = Sympa::Template->new;
    $template->parse($data, $tpl_file, \$output);

# DESCRIPTION

## Methods

- new ( $that, \[ property defaults \] )

    _Constructor_.
    Creates new [Sympa::Template](./Sympa-Template.3.md) instance.

    Parameters:

    - $that

        Context.  Site, Robot or List.

    - property defaults

        Pairs to specify property defaults.

- parse ( $data, $tpl, $output, \[ has\_header => 1 \] )

    _Instance method_.
    Parses template and outputs result.

    Parameters:

    - $data

        A HASH ref containing the data.

    - $tpl

        A string that contains the file name.
        Or, scalarref or arrayref that contains the template.

    - $output

        A file descriptor or a reference to scalar for the output.

    - has\_header => 0|1

        If 1 is set, prepended header fields are assumed,
        i.e. one newline will be inserted at beginning of output.

    - is\_not\_template => 0|1

        This option was obsoleted.

    Returns:

    On success, returns `1` and clears {last\_error} property.
    Otherwise returns `undef` and sets {last\_error} property.

## Properties

Instance of [Sympa::Template](./Sympa-Template.3.md) may have following attributes.

- {allow\_absolute}

    If set, absolute paths in `INCLUDE` directive are allowed.

- {include\_path}

    Reference to array containing additional template search paths.

- {last\_error}

    _Read only_.
    Error occurred at the last execution of parse, or `undef`.

- {subdir}, {lang}, {lang\_only}

    TBD.

## Filters

These custom filters are defined by [Sympa::Template](./Sympa-Template.3.md).
See [Template::Manual::Filters](https://metacpan.org/pod/Template::Manual::Filters) about usage of filters.

- canonic\_email

    Canonicalize e-mail address.

    This filter was added by Sympa 6.2.17.

- decode\_utf8

    No longer used.

- encode\_utf8

    No longer used.

- escape\_cstr

    Applies C-style escaping of a string (not enclosed by quotes).

    This filter was added on Sympa 6.2.37b.1.

- escape\_quote

    Escape quotation marks.

    **Deprecated**.
    Use escape\_cstr.

- escape\_url

    Escapes URL.

    This was OBSOLETED.  Use ["mailtourl"](#mailtourl) instead.

- escape\_xml

    OBSOLETED.  Use ["xml" in Template::Manual::Filters](https://metacpan.org/pod/Template::Manual::Filters#xml).

- helploc ( parameters )
- l ( parameters )
- loc ( parameters )

    Translates text using catalog.
    Placeholders (`%1`, `%2`, ...) are replaced by parameters.

- locdt ( argument )

    Generates formatted (i18n'ized) date/time.

    - Filtered text

        strftime() style format string.

    - argument

        A string representing date/time:
        "YYYY/MM", "YYYY/MM/DD", "YYYY/MM/DD/HH/MM" or "YYYY/MM/DD/HH/MM/SS".

- mailto ( email, \[ {key => val, ...}, \[ nodecode \] \] )

    Generates HTML fragment linking to `mailto:` URL,
    i.e. `<a href="mailto:_email_">_filtered text_</a>`.

    - Filtered text

        Content of linking element.
        If it does not contain nonspaces, e-mail address will be used.

    - email

        E-mail address(es) to be linked.

    - {key => val, ...}

        Optional query.

    - nodecode

        If true, assumes arguments are not encoded as HTML entities.
        By default entities are decoded at first.

        This option does _not_ affect filtered text.

    Note:
    This filter was introduced by Sympa 6.2.14.

- mailtourl ( \[ {key = val, ...} \] )

    Generates `mailto:` URL.

    - Filtered text

        E-mail address(es).
        Note that any characters must not be encoded as HTML entities.

    - {key = val, ...}

        Optional query.
        Note that any characters must not be encoded as HTML entities.

    Note:
    This filter was introduced by Sympa 6.2.14.

- obfuscate ( mode )

    Obfuscates email addresses in the HTML text according to mode.

    - Filtered text

        HTML document or fragment.

    - mode

        Obfuscation mode.  `at` or `javascript`.
        Invalid mode will be silently ignored.

    Note:
    This filter was introduced by Sympa 6.2.14.

- optdesc ( type, withval )

    Generates i18n'ed description of list parameter value.

    As of Sympa 6.2.17, if it is called by the web templates
    (in `web_tt2` subdirectories),
    special characters in result will be encoded.

    - Filtered text

        Parameter value.

    - type

        Type of list parameter value:
        Special types (See ["field\_type" in Sympa::ListDef](./Sympa-ListDef.3.md#field_type))
        or others (default).

    - withval

        If parameter value is added to the description.  False by default.

- qencode

    Encode string by MIME header encoding.
    Despite its name, appropriate encoding scheme
    (`Q` or `B`) will be chosen.

- unescape

    No longer used.

- url\_abs ( ... )

    Same as ["url\_rel"](#url_rel) but gives absolute URI.

    Note:
    This filter was introduced by Sympa 6.2.15.

- url\_rel ( \[ paths, \[ query, \[ fragment \] \] \] )

    Gives relative URI for specified action.

    If it is called by the web templates (in `web_tt2` subdirectories),
    HTML entities in the arguments will be decoded
    and special characters in resulting URL will be encoded.

    - Filtered text

        Name of action.

    - paths

        Array.  Additional path components.

    - query

        Hash.  Optional query.

    - fragment

        Scalar.  Optional fragment.

    Note:
    This filter was introduced by Sympa 6.2.15.

- wrap ( init, subs, cols )

    Generates folded text.

    - init

        Indentation (or its length) of each paragraph if any.

    - subs

        Indentation (or its length) of other lines if any.

    - cols

        Line width, defaults to 78.

**Note**:

Calls of ["helploc"](#helploc), ["loc"](#loc) and ["locdt"](#locdt) in template files are
extracted during packaging process and are added to translation catalog.

## Plugins

Plugins may be placed under `LIBDIR/Sympa/Template/Plugin`.
See &lt;https://sympa-community.github.io/manual/customize/template-plugins.html>
about usage of plugins.

# SEE ALSO

[Template::Manual](https://metacpan.org/pod/Template::Manual).

# HISTORY

Sympa 4.2b.3 adopted template engine based on Template Toolkit.

Plugin feature was added on Sympa 6.2.

[tt2](https://metacpan.org/pod/tt2) module was renamed to [Sympa::Template](./Sympa-Template.3.md) on Sympa 6.2.
