---
title: 'Sympa::HTMLDecorator(3)'
---

# NAME

Sympa::HTMLDecorator - Decorating HTML texts

# SYNOPSIS

    use Sympa::HTMLDecorator;
    $decorator = Sympa::HTMLDecorator->instance;
    $ouput = $decorator->decorate($html, email => 'javascript');

# DESCRIPTION

[Sympa::HTMLDecorator](./Sympa-HTMLDecorator.3.md) transforms HTML texts.

## Methods

- instance ( )

    _Constructor_.
    Returns singleton instance of this class.

- decorate ( $html, email => $mode )

    _Instance method_.
    Modifies HTML text.

    Parameters:

    - $html

        A text including HTML document or fragment.
        It must be encoded by UTF-8.

    - email => $mode

        Transformation mode.
        `'at'` replaces `@` in email addresses.
        `'javascript'` obfuscates emails using JavaScript code.

    Returns:

    Modified text.

# SEE ALSO

[Sympa::HTMLSanitizer](./Sympa-HTMLSanitizer.3.md).

# HISTORY

[Sympa::HTMLDecorator](./Sympa-HTMLDecorator.3.md) appeared on Sympa 6.2.14.
