# NAME

charset.conf - Configiration file for legacy character set support by Sympa

# DESCRIPTION

In some language environments, legacy encoding (character set) is
preferred for e-mail messages: For example `iso-2022-jp` in Japanese
language.

`charset.conf` defines mapping between language context and legacy character
set for service messages sent by Sympa.  If you want to enable legacy character
set support, simply copy sample `charset.conf` onto configuration
directory:

    # cp /home/sympa/default/charset.conf /home/sympa/etc/charset.conf

And set the `legacy_character_support_feature` parameter value in
`/etc/sympa/sympa.conf` to `on`.

## Note

If you are planning to upgrade Sympa earlier than 5.3a.8,
original `charset.conf` is required to convert shared documents
during upgrade process.

# SEE ALSO

[sympa.conf(5)](./sympa.conf.5.md).

# HISTORY

Legacy character set support appeared on Sympa 6.0.

This document was initially written by IKEDA Soji <ikeda@conversion.co.jp>
at 16, Mar 2009,
and modified by VERDIN David <david.verdin@cru.fr>
at 27, April 2009.
