---
title: 'Web interface: Miscellaneous modules'
up: ../customize.md#web-interface-optional-features
---

Web interface: Miscellaneous modules
====================================

Recommended Perl modules
------------------------

Installing these Perl modules is recommended to make Sympa more
reliable.

  * [Unicode-Normalize](https://metacpan.org/release/Unicode-Normalize)

    Normalizes file names represented by Unicode.
    This will fix file names with decomposition form created on
    some platforms such as macOS.

Perl modules for migration from earlier releases
------------------------------------------------

Following modules may be required if you are planning to migrate an
earlier release of Sympa.  They are not necessary for recent releases.

  * [Crypt-CipherSaber](https://metacpan.org/release/Crypt-CipherSaber)

    For releases from 3.1 to 5.x:
    Provides RC4 algorithm to decrypt passwords stored in database.
