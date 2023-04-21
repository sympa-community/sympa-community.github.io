---
assets:
  - created_at: 2021-10-23T08:24:46Z
    name: sympa-6.2.67b.1.tar.gz
    size: 12961313
    updated_at: 2021-10-23T08:24:49Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.67b.1/sympa-6.2.67b.1.tar.gz
  - created_at: 2021-10-23T08:24:49Z
    name: sympa-6.2.67b.1.tar.gz.md5
    size: 57
    updated_at: 2021-10-23T08:24:49Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.67b.1/sympa-6.2.67b.1.tar.gz.md5
  - created_at: 2021-10-23T08:24:44Z
    name: sympa-6.2.67b.1.tar.gz.sha256
    size: 89
    updated_at: 2021-10-23T08:24:45Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.67b.1/sympa-6.2.67b.1.tar.gz.sha256
  - created_at: 2021-10-23T08:24:45Z
    name: sympa-6.2.67b.1.tar.gz.sha512
    size: 161
    updated_at: 2021-10-23T08:24:46Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.67b.1/sympa-6.2.67b.1.tar.gz.sha512
created_at: 2021-10-23T08:13:30Z
prerelease: 1
published_at: 2021-10-24T07:56:18Z
tag_name: 6.2.67b.1
title: Sympa 6.2.67 beta released
---

<img align="right" src="https://assets.sympa.community/logos/sympa_beta.png" title="Sympa beta logo"/> 24 October 2021

The Sympa Community is proud to release the new beta of the next version of Sympa. Please install it to test and report bugs, translate user interface to your language, or enhance documentation of Sympa, if you want to help the Sympa community deliver a more reliable version of Sympa.

**Sympa 6.2.67b.1** is the new beta version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.67b.1/sympa-6.2.67b.1.tar.gz)

  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.67b.1/NEWS.md)

  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)

  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.67b.1/CONTRIBUTING.md)

This version introduces some improvements and fixes several bugs.

About beta version
---------------------  

This version is a pre-release version of the next stable release.  We expect feedback from users and developers.  To know how to report bugs or improvements, see [Contributing to Sympa](https://github.com/sympa-community/sympa/blob/6.2.67b.1/CONTRIBUTING.md).

  - Next stable release, 6.2.68, is planned to be released in **late Decdember 2021**.

  - Translations added on [translation site](https://translate.sympa.org/) before 17th December 00:00 UTC will be shipped with the stable release.

### Significant improvements and bug fixes

  - The new option `--del` to remove emails from list using `sympa.pl` command line interface [\#911](https://github.com/sympa-community/sympa/pull/911).
  - Fixed memory consumption while archive download [\#1235](https://github.com/sympa-community/sympa/issues/1235).  This fix requires new Perl modules: Check `cpanfile` file.

For details on the other changes see [the release notes](https://github.com/sympa-community/sympa/blob/6.2.67b.1/NEWS.md).

----
[The Sympa Community](https://github.com/sympa-community)
