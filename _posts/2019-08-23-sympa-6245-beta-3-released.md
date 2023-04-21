---
assets:
  - created_at: 2019-08-23T09:02:52Z
    name: sympa-6.2.45b.3.tar.gz
    size: 12762171
    updated_at: 2019-08-23T09:04:53Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.45b.3/sympa-6.2.45b.3.tar.gz
  - created_at: 2019-08-23T09:07:57Z
    name: sympa-6.2.45b.3.tar.gz.md5
    size: 57
    updated_at: 2019-08-23T09:07:59Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.45b.3/sympa-6.2.45b.3.tar.gz.md5
  - created_at: 2019-08-23T09:08:06Z
    name: sympa-6.2.45b.3.tar.gz.sha256
    size: 89
    updated_at: 2019-08-23T09:08:07Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.45b.3/sympa-6.2.45b.3.tar.gz.sha256
created_at: 2019-08-23T08:52:21Z
prerelease: 1
published_at: 2019-08-23T12:25:53Z
tag_name: 6.2.45b.3
title: Sympa 6.2.45 beta 3 released
---

<img align="right" src="https://assets.sympa.community/logos/sympa_beta.png" title="Sympa beta logo"/> 23 August 2019

The Sympa Community is proud to release the new beta of the next version of Sympa. Please install it to test and report bugs, translate user interface to your language, or enhance documentation of Sympa, if you want to help the Sympa community deliver a more reliable version of Sympa.

**Sympa 6.2.45b.3** is the new beta version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.45b.3/sympa-6.2.45b.3.tar.gz)

  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.45b.3/NEWS.md)

  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)

  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.45b.3/CONTRIBUTING.md)

This version introduces some improvements and fixes several bugs.

About beta version
---------------------  

This version is a pre-release version of the next stable release.  We expect feedback from users and developers.  To know how to report bugs or improvements, see [Contributing to Sympa](https://github.com/sympa-community/sympa/blob/6.2.45b.3/CONTRIBUTING.md).

  - Next stable release, 6.2.46, is planned to be released on **Monday, 23rd September 2019** (in UTC).

Translation catalog was updated because some messages were added mainly for new features.

  - Translations added on [translation site](https://translate.sympa.org/) before 17th September 00:00 UTC will be shipped with the stable release.

### Significant changes

  * Data sources: Codebase has entirely been rewritten. Some behaviors will be changed.  **If you noticed any problem, please let us know** on issue page [\#693](https://github.com/sympa-community/sympa/issues/693).

  * Perl: From now on, Perl earlier than 5.10.1 will never be supported.  See also [\#620](https://github.com/sympa-community/sympa/issues/620).

Packaging
-----------

### RPM: Sympa is joining to Fedora/EPEL

Xavier Bachelot has been working to add Sympa to Fedora/EPEL for years.  Now it is coming true.

To install Sympa packages under tests on Fedora/EPEL, you have to enable "testing" repository, like:
``` bash
dnf install --enablerepo=updates-testing sympa
```
or, with EPEL,
``` bash
yum install --enablerepo=epel-testing sympa
```
In near future, RPM will be provided through these repositories.  Until then, administrators who need stable RPM packages may use older repository, `Sympa-JA.org`.  RPMs for pre-release (beta) vdersion of Sympa are provided by COPR `sympa-beta` repository (See [the documentation](https://sympa-community.github.io/manual/install/install-sympa-distribution-rpm.html) for details).

----
[The Sympa Community](https://github.com/sympa-community)

