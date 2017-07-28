# NAME

Sympa::Tools::Text - Text-related functions

# DESCRIPTION

This package provides some text-related functions.

## Functions

- addrencode ( $addr, \[ $phrase, \[ $charset, \[ $comment \] \] \] )

    Returns formatted (and encoded) name-addr as RFC5322 3.4.

- canonic\_email ( $email )

    _Function_.
    Returns canonical form of e-mail address.

    Leading and trailing whilte spaces are removed.
    Latin letters without accents are lower-cased.

    For malformed inputs returns `undef`.

- canonic\_message\_id ( $message\_id )

    Returns canonical form of message ID without trailing or leading whitespaces
    or `<`, `>`.

- decode\_filesystem\_safe ( $str )

    _Function_.
    Decodes a string encoded by encode\_filesystem\_safe().

    Parameter:

    - $str

        String to be decoded.

    Returns:

    Decoded string, stripped `utf8` flag if any.

- decode\_html ( $str )

    _Function_.
    Decodes HTML entities in a string encoded by UTF-8 or a Unicode string.

    Parameter:

    - $str

        String to be decoded.

    Returns:

    Decoded string, stripped `utf8` flag if any.

- encode\_filesystem\_safe ( $str )

    _Function_.
    Encodes a string $str to be suitable for filesystem.

    Parameter:

    - $str

        String to be encoded.

    Returns:

    Encoded string, stripped `utf8` flag if any.
    All bytes except `'-'`, `'+'`, `'.'`, `'@'`
    and alphanumeric characters are encoded to sequences `'_'` followed by
    two hexdigits.

    Note that `'/'` will also be encoded.

- encode\_html ( $str )

    _Function_.
    Encodes characters in a string $str to HTML entities.
    `'<'`, `'>'`, `'&'` and `'"'` are encoded.

    Parameter:

    - $str

        String to be encoded.

    Returns:

    Encoded string, _not_ stripping utf8 flag if any.

- encode\_uri ( $str, \[ omit => $chars \] )

    _Function_.
    Encodes potentially unsafe characters in the string using "percent" encoding
    suitable for URIs.

    Parameters:

    - $str

        String to be encoded.

    - omit => $chars

        By default, all characters except those defined as "unreserved" in RFC 3986
        are encoded, that is, `[^-A-Za-z0-9._~]`.
        If this parameter is given, it will prevent encoding additional characters.

    Returns:

    Encoded string, stripped `utf8` flag if any.

- escape\_chars ( $str )

    Escape weird characters.

    ToDo: This should be obsoleted in the future release: Would be better to use
    ["encode\_filesystem\_safe"](#encode_filesystem_safe).

- escape\_url ( $str )

    DEPRECATED.
    Would be better to use ["encode\_uri"](#encode_uri) or ["mailtourl"](#mailtourl).

- foldcase ( $str )

    _Function_.
    Returns "fold-case" string suitable for case-insensitive match.
    For example, a code below looks for a needle in haystack not regarding case,
    even if they are non-ASCII UTF-8 strings.

        $haystack = Sympa::Tools::Text::foldcase($HayStack);
        $needle   = Sympa::Tools::Text::foldcase($NeedLe);
        if (index $haystack, $needle >= 0) {
            ...
        }

    Parameter:

    - $str

        A string.

- guessed\_to\_utf8( $text, \[ lang, ... \] )

    _Function_.
    Guesses text charset considering language context
    and returns the text reencoded by UTF-8.

    Parameters:

    - $text

        Text to be reencoded.

    - lang, ...

        Language tag(s) which may be given by ["implicated\_langs" in Sympa::Language](./Sympa::Language.3.md#implicated_langs).

    Returns:

    Reencoded text.
    If any charsets could not be guessed, `iso-8859-1` will be used
    as the last resort, just because it covers full range of 8-bit.

- mailtourl ( $email, \[ decode\_html => 1 \],
\[ query => {key => val, ...} \] )

    _Function_.
    Constructs a `mailto:` URL for given e-mail.

    Parameters:

    - $email

        E-mail address.

    - decode\_html => 1

        If set, arguments are assumed to include HTML entities.

    - query => {key => val, ...}

        Optional query.

    Returns:

    Constructed URL.

- pad ( $str, $width )

    Pads space a string so that result will not be narrower than given width.

    Parameters:

    - $str

        A string.

    - $width

        If $width is false value or width of $str is not less than $width,
        does nothing.
        If $width is less than `0`, pads right.
        Otherwise, pads left.

    Returns:

    Padded string.

- qdecode\_filename ( $filename )

    Q-Decodes web file name.

    ToDo:
    This should be obsoleted in the future release: Would be better to use
    ["decode\_filesystem\_safe"](#decode_filesystem_safe).

- qencode\_filename ( $filename )

    Q-Encodes web file name.

    ToDo:
    This should be obsoleted in the future release: Would be better to use
    ["encode\_filesystem\_safe"](#encode_filesystem_safe).

- unescape\_chars ( $str )

    Unescape weird characters.

    ToDo: This should be obsoleted in the future release: Would be better to use
    ["decode\_filesystem\_safe"](#decode_filesystem_safe).

- valid\_email ( $string )

    Basic check of an email address.

- weburl ( $base, \\@paths, \[ decode\_html => 1 \],
\[ fragment => $fragment \], \[ query => \\%query \] )

    Constructs a `http:` or `https:` URL under given base URI.

    Parameters:

    - $base

        Base URI.

    - \\@paths

        Additional path components.

    - decode\_html => 1

        If set, arguments are assumed to include HTML entities.
        Exception is $base:
        It is assumed not to include entities.

    - fragment => $fragment

        Optional fragment.

    - query => \\%query

        Optional query.

    Returns:

    A URI.

- wrap\_text ( $text, \[ $init\_tab, \[ $subsequent\_tab, \[ $cols \] \] \] )

    _Function_.
    Returns line-wrapped text.

    Parameters:

    - $text

        The text to be folded.

    - $init\_tab

        Indentation prepended to the first line of paragraph.
        Default is `''`, no indentation.

    - $subsequent\_tab

        Indentation prepended to each subsequent line of folded paragraph.
        Default is `''`, no indentation.

    - $cols

        Max number of columns of folded text.
        Default is `78`.

# HISTORY

[Sympa::Tools::Text](./Sympa::Tools::Text.3.md) appeared on Sympa 6.2a.41.

decode\_filesystem\_safe() and encode\_filesystem\_safe() were added
on Sympa 6.2.10.

decode\_html(), encode\_html(), encode\_uri() and mailtourl()
were added on Sympa 6.2.14, and escape\_url() was deprecated.

guessed\_to\_utf8() and pad() were added on Sympa 6.2.17.
