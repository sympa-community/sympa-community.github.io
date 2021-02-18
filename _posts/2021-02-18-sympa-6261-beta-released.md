---
assets:
  - created_at: 2021-02-18T03:24:57Z
    name: sympa-6.2.61b.1.tar.gz
    size: 12891990
    updated_at: 2021-02-18T03:24:59Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.61b.1/sympa-6.2.61b.1.tar.gz
  - created_at: 2021-02-18T03:24:55Z
    name: sympa-6.2.61b.1.tar.gz.md5
    size: 57
    updated_at: 2021-02-18T03:24:56Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.61b.1/sympa-6.2.61b.1.tar.gz.md5
  - created_at: 2021-02-18T03:24:56Z
    name: sympa-6.2.61b.1.tar.gz.sha256
    size: 89
    updated_at: 2021-02-18T03:24:56Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.61b.1/sympa-6.2.61b.1.tar.gz.sha256
  - created_at: 2021-02-18T03:24:56Z
    name: sympa-6.2.61b.1.tar.gz.sha512
    size: 161
    updated_at: 2021-02-18T03:24:57Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.61b.1/sympa-6.2.61b.1.tar.gz.sha512
created_at: 2021-02-18T03:03:59Z
prerelease: 1
published_at: 2021-02-18T03:32:44Z
tag_name: 6.2.61b.1
title: Sympa 6.2.61 beta released
---

<img align="right" src="https://www.sympa.org/_media/logos/old/sympa_beta.png" title="Sympa beta logo"/> 18 February 2021

The Sympa Community is proud to release the new beta of the next version of Sympa. Please install it to test and report bugs, translate user interface to your language, or enhance documentation of Sympa, if you want to help the Sympa community deliver a more reliable version of Sympa.

**Sympa 6.2.61b.1** is the new beta version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.61b.1/sympa-6.2.61b.1.tar.gz)

  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.61b.1/NEWS.md)

  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)

  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.61b.1/CONTRIBUTING.md)

This version introduces some improvements and fixes several bugs.

About beta version
---------------------  

This version is a pre-release version of the next stable release.  We expect feedback from users and developers.  To know how to report bugs or improvements, see [Contributing to Sympa](https://github.com/sympa-community/sympa/blob/6.2.61b.1/CONTRIBUTING.md).

  - Next stable release, 6.2.62, is planned to be released on **Saturday, 20th March 2021** (in UTC).

Translation catalog was updated because some messages were added mainly for new features.

  - Translations added on [translation site](https://translate.sympa.org/) before 13th March 00:00 UTC will be shipped with the stable release.

### Significant changes

  * The parameter "`cookie`" was obsoleted [\#1091](https://github.com/sympa-community/sympa/issues/1091).

  * MHonArc resource template `mhonarc-ressources.tt2` was renamed to `mhonarc_rc.tt2` and format of tags in it was changed.  Existing customizations will be migrated during upgrading process [\#1095](https://github.com/sympa-community/sympa/pull/1095).

  * By a request from the user with English background, the term "blacklist" was replaced with "blocklist".  By this change, names of some parameters and configuration file were changed [\#1111](https://github.com/sympa-community/sympa/issues/1111). This change will be migrated automatically during upgrading process.

  * Drop support for Perl prior to 5.16 [\#1030](https://github.com/sympa-community/sympa/issues/1030).

----
[The Sympa Community](https://github.com/sympa-community)
