---
title: 'Sympa::HTMLSanitizer(3)'
---

# NAME

Sympa::HTMLSanitizer - Sanitize HTML contents

# SYNOPSIS

    $hss = Sympa::HTMLSanitizer->new;

    $sanitized = $hss->sanitize_html($html);
    $sanitized = $hss->sanitize_html_file($file);
    $hss->sanitize_var($variable);

# DESCRIPTION

TBD.

## Methods

- new ( $robot )

    _Constructor_.
    Creates a new [Sympa::HTMLSanitizer](./Sympa-HTMLSanitizer.3.md) instance.

    Parameter:

    - $robot

        Robot context to determine allowed URL prefix.

    Returns:

    New [Sympa::HTMLSanitizer](./Sympa-HTMLSanitizer.3.md) instance.

- sanitize\_html ( $html )

    _Instance method_.
    Returns sanitized version of HTML source.

    Parameter:

    - $html

        HTML source.

    Returns:

    Sanitized source.

- sanitize\_html\_file ( $file )

    _Instance method_.
    Returns sanitized version of HTML source in the file.

    Parameter:

    - $file

        HTML file.

    Returns:

    Sanitized source.

- sanitize\_var ( $var, \[ options... \] )

    _Instance method_.
    Sanitize all items in hashref or arrayref recursively.

    TBD.

# HISTORY

TBD.
