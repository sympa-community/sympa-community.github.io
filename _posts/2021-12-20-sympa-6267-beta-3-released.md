---
assets:
  - created_at: 2021-12-20T05:03:32Z
    name: sympa-6.2.67b.3.tar.gz
    size: 12994216
    updated_at: 2021-12-20T05:03:36Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.67b.3/sympa-6.2.67b.3.tar.gz
  - created_at: 2021-12-20T05:03:36Z
    name: sympa-6.2.67b.3.tar.gz.md5
    size: 57
    updated_at: 2021-12-20T05:03:37Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.67b.3/sympa-6.2.67b.3.tar.gz.md5
  - created_at: 2021-12-20T05:03:37Z
    name: sympa-6.2.67b.3.tar.gz.sha256
    size: 89
    updated_at: 2021-12-20T05:03:37Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.67b.3/sympa-6.2.67b.3.tar.gz.sha256
  - created_at: 2021-12-20T05:03:37Z
    name: sympa-6.2.67b.3.tar.gz.sha512
    size: 161
    updated_at: 2021-12-20T05:03:38Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.67b.3/sympa-6.2.67b.3.tar.gz.sha512
created_at: 2021-12-20T04:41:15Z
prerelease: 1
published_at: 2021-12-20T05:06:11Z
tag_name: 6.2.67b.3
title: Sympa 6.2.67 beta 3 released
---

<img align="right" src="https://www.sympa.org/_media/logos/old/sympa_beta.png" title="Sympa beta logo"/> 20 December 2021

The Sympa Community is proud to release the new beta of the next version of Sympa. Please install it to test and report bugs, translate user interface to your language, or enhance documentation of Sympa, if you want to help the Sympa community deliver a more reliable version of Sympa.

**Sympa 6.2.67b.3** is the new beta version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.67b.3/sympa-6.2.67b.3.tar.gz)

  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.67b.3/NEWS.md)

  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)

  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.67b.3/CONTRIBUTING.md)

This version introduces some improvements and fixes several bugs.

About beta version
---------------------  

This version is a pre-release version of the next stable release.  We expect feedback from users and developers.  To know how to report bugs or improvements, see [Contributing to Sympa](https://github.com/sympa-community/sympa/blob/6.2.67b.3/CONTRIBUTING.md).

  - Next stable release, 6.2.68, is planned to be released in **early January 2022**.

Translation catalog was updated because some messages were added mainly for new features.

  - Translations added on [translation site](https://translate.sympa.org/) before 31st December 00:00 UTC will be shipped with the stable release.

### Significant improvements and bug fixes

  - Improvement of command line format of `sympa.pl` utility [\#1303](https://github.com/sympa-community/sympa/issues/1303).  Previous format will also be available for the time being.
  - The new option `--add` and `--del` for `sympa.pl` command line utilityt. These options also may add/remove list owners and moderators along with list subscribers [\#911](https://github.com/sympa-community/sympa/pull/911) [\#1251](https://github.com/sympa-community/sympa/pull/1251). The option `--import` was deprecated.
  - Fixed memory consumption while archive download [\#1235](https://github.com/sympa-community/sympa/issues/1235).  This fix requires new Perl modules: Check `cpanfile` file.

For details on the other changes see [the release notes](https://github.com/sympa-community/sympa/blob/6.2.67b.3/NEWS.md).

----
[The Sympa Community](https://github.com/sympa-community)
