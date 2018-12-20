---
title: 'Sympa services: Miscellaneous modules'
up: ../customize.md#sympa-services-optional-features
---

Sympa services: Miscellaneous modules
=====================================

Recommended Perl modules
------------------------

Installing these Perl modules is recommended to make Sympa more
reliable.

  * [Clone](https://metacpan.org/release/Clone)

    Used to make copy of internal data structures.
    Installing this will make Sympa working faster.

  * [Encode-Locale](https://metacpan.org/release/Encode-Locale)

    Useful when running command line utilities in the console not
    supporting UTF-8 encoding.

  * Encode modules for extended legacy character sets

    These allow Sympa to handle messages using legacy character set
    better.  These can be necessary in specific language contexts.

      - [Encode-EUCJPASCII](https://metacpan.org/release/Encode-EUCJPASCII)
        - Japanese

      - [Encode-HanExtra](https://metacpan.org/release/Encode-HanExtra)
        - Chinese

