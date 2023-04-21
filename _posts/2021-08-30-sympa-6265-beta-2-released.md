---
assets:
  - created_at: 2021-08-30T06:26:50Z
    name: sympa-6.2.65b.2.tar.gz
    size: 12943800
    updated_at: 2021-08-30T06:28:02Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.65b.2/sympa-6.2.65b.2.tar.gz
  - created_at: 2021-08-30T06:28:02Z
    name: sympa-6.2.65b.2.tar.gz.md5
    size: 57
    updated_at: 2021-08-30T06:28:03Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.65b.2/sympa-6.2.65b.2.tar.gz.md5
  - created_at: 2021-08-30T06:26:47Z
    name: sympa-6.2.65b.2.tar.gz.sha256
    size: 89
    updated_at: 2021-08-30T06:26:49Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.65b.2/sympa-6.2.65b.2.tar.gz.sha256
  - created_at: 2021-08-30T06:26:49Z
    name: sympa-6.2.65b.2.tar.gz.sha512
    size: 161
    updated_at: 2021-08-30T06:26:50Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.65b.2/sympa-6.2.65b.2.tar.gz.sha512
created_at: 2021-08-30T06:09:04Z
prerelease: 1
published_at: 2021-08-30T06:35:43Z
tag_name: 6.2.65b.2
title: Sympa 6.2.65 beta 2 released
---

<img align="right" src="https://assets.sympa.community/logos/sympa_beta.png" title="Sympa beta logo"/> 30 August 2021

The Sympa Community is proud to release the new beta of the next version of Sympa. Please install it to test and report bugs, translate user interface to your language, or enhance documentation of Sympa, if you want to help the Sympa community deliver a more reliable version of Sympa.

**Sympa 6.2.65b.2** is the new beta version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.65b.2/sympa-6.2.65b.2.tar.gz)

  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.65b.2/NEWS.md)

  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)

  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.65b.2/CONTRIBUTING.md)

This version introduces some improvements and fixes several bugs.

About beta version
---------------------  

This version is a pre-release version of the next stable release.  We expect feedback from users and developers.  To know how to report bugs or improvements, see [Contributing to Sympa](https://github.com/sympa-community/sympa/blob/6.2.65b.2/CONTRIBUTING.md).

  - Next stable release, 6.2.66, is planned to be released in **late September 2021**.

Translation catalog was updated because some messages were added mainly for new features.

  - Translations added on [translation site](https://translate.sympa.org/) before 15th September 00:00 UTC will be shipped with the stable release.

### Significant changes

  * **Improving data source synchronization performance** ([\#1186](https://github.com/sympa-community/sympa/issues/1186))

    The sync_include (synchronization of list users with data sources) process was optimized. Some benchmarks show a 3x reduction of processing time.  More tests are desired.

  * Now the characters used for e-mail addresses conform to RFC 5322 ([#1217](https://github.com/sympa-community/sympa/issues/1217))

    Some characters such as backtick are allowed in the e-mail addresses of the users.

For details on the other changes see [the release notes](https://github.com/sympa-community/sympa/blob/6.2.65b.2/NEWS.md).

----
[The Sympa Community](https://github.com/sympa-community)
