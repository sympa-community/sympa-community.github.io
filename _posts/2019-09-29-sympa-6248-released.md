---
assets:
  - created_at: 2019-09-29T09:18:36Z
    name: sympa-6.2.48.tar.gz
    size: 12768937
    updated_at: 2019-09-29T09:20:33Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.48/sympa-6.2.48.tar.gz
  - created_at: 2019-09-29T09:20:55Z
    name: sympa-6.2.48.tar.gz.md5
    size: 54
    updated_at: 2019-09-29T09:20:56Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.48/sympa-6.2.48.tar.gz.md5
  - created_at: 2019-09-29T09:21:14Z
    name: sympa-6.2.48.tar.gz.sha256
    size: 86
    updated_at: 2019-09-29T09:21:15Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.48/sympa-6.2.48.tar.gz.sha256
created_at: 2019-09-28T09:47:11Z
prerelease: 0
published_at: 2019-09-29T09:00:50Z
tag_name: 6.2.48
title: Sympa 6.2.48 released
---

<img align="right" src="https://assets.sympa.community/logos/sympa_multi_150x121.png" title="Sympa logo"/> 29 September 2019

The Sympa Community is proud to release the newest version of Sympa.

**Sympa 6.2.48** is the newest stable version of Sympa 6.2 (Note that 6.2.46 has been withdrawn).

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.48/sympa-6.2.48.tar.gz)
  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.48/NEWS.md)
  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)
  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.48/CONTRIBUTING.md)

This version fixes several bugs, and introduces some enhancements.  Translations to several languages have been mostly completed.  Administrators are encouraged to upgrade Sympa to this version.

Highlight of this version
-------------------------

### Significant changes

  * Data sources: Codebase has entirely been rewritten. It will work a bit faster with less memory usage in exchange for some changes on behaviors.
  * Perl: From now on, Perl earlier than 5.10.1 will never be supported.

### Internationalization

Thanks to heavy works by translators on [translation site](https://translate.sympa.org), Sympa almost completely supports following languages:

  * German (Deutsch)
  * Spanish (Español)
  * French (Français)
  * Galician (Galego)
  * Italian (Italiano)
  * Japanese (日本語)
  * Russian (Русский)
  * US English

Along with languages above, help documents for users are provided in following languages:

  * Catalan (Català)
  * Basque (Euskara)
  * Polish (Polski)

Packaging
-----------

### FreeBSD

Currently, FreeBSD ports/packages provides [`mail/sympa`](https://www.freebsd.org/cgi/ports.cgi?query=sympa&sektion=mail) with Sympa 6.2.44.

### RPM: Sympa is available in both Fedora and EPEL

Xavier Bachelot has been working to add Sympa to Fedora and EPEL (Extras packages for Enterprise Linux) for years.  From now on, sympa RPMs will be provided through these repositories.

To install Sympa packages on Fedora, use :
``` bash
dnf install sympa
```
For RHEL/CentOS with the additional EPEL repository, use :
``` bash
yum install sympa
```

RPMs for pre-release (beta) versions of Sympa are provided in a  COPR `sympa-beta` repository (See [the documentation](https://sympa-community.github.io/manual/install/install-sympa-distribution-rpm.html) for details).

All current Fedora versions and RHEL/CentOS 6 and 7 are supported. However, please note sympa is not yet available for RHEL/CentOS 8, this is still a work in progress.

Planned changes in the future
-----------------------------

### New Sympa logo

New Sympa logo is proposed.  If you have any idea, please comment on the issue [\#665](https://github.com/sympa-community/sympa/issues/665).

----
[The Sympa Community](https://github.com/sympa-community)
