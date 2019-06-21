---
assets:
  - created_at: 2019-06-21T06:05:04Z
    name: sympa-6.2.44.tar.gz
    size: 13051456
    updated_at: 2019-06-21T06:07:10Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.44/sympa-6.2.44.tar.gz
  - created_at: 2019-06-21T06:07:19Z
    name: sympa-6.2.44.tar.gz.md5
    size: 54
    updated_at: 2019-06-21T06:07:20Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.44/sympa-6.2.44.tar.gz.md5
  - created_at: 2019-06-21T06:07:26Z
    name: sympa-6.2.44.tar.gz.sha256
    size: 86
    updated_at: 2019-06-21T06:07:27Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.44/sympa-6.2.44.tar.gz.sha256
created_at: 2019-06-21T03:28:22Z
prerelease: 0
published_at: 2019-06-21T13:03:09Z
tag_name: 6.2.44
title: Sympa 6.2.44 released
---

<img align="right" src="https://www.sympa.org/_media/logos/old/sympa_multi_150x121.png" title="Sympa logo"/> 21 June 2019

The Sympa Community is proud to release the newest version of Sympa.

**Sympa 6.2.44** is the newest stable version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.44/sympa-6.2.44.tar.gz)
  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.44/NEWS.md)
  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)
  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.44/CONTRIBUTING.md)

This version fixes several bugs, and introduces some enhancements including new features described in below.  Translations to several languages have been mostly completed.  Administrators are encouraged to upgrade Sympa to this version.

Highlight of this version
-------------------------

### New features

Here are some of remarklable enhancements on this release.  See also [release notes](https://github.com/sympa-community/sympa/blob/6.2.44/NEWS.md) to know about all of improvements.

  - Hide full email addresses in archives [\#621](https://github.com/sympa-community/sympa/issues/621) ([ldidry](https://github.com/ldidry))

    A new option `gecos` for "email address protection method" (`web_archive_spam_protection`) which allows to hide e-mail in archives entirely.

  - Button for full export of subscribers [\#616](https://github.com/sympa-community/sympa/pull/616) ([ldidry](https://github.com/ldidry))

    With "Dump with names" button, list owners may export subscribers with their display names.

  - Admin function to bulk unsubscribe [\#27](https://github.com/sympa-community/sympa/issues/27) ([ldidry](https://github.com/ldidry))

    This feature has been implemented on 6.1.x, dropped on 6.2 and now restored.

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

### Debian

At last Debian 10 "buster" will be [released on 2019-07-06](https://wiki.debian.org/ReleasePartyBuster)!  It will provide [`sympa`](https://packages.debian.org/buster/sympa) package with Sympa 6.2.40.

### FreeBSD

Ports/packages provides [`mail/sympa`](https://www.freebsd.org/cgi/ports.cgi?query=sympa&sektion=mail) with Sympa 6.2.42.

### RPM

People running either Fedora or RHEL (and clones) can find ready to use RPMs for sympa in a [COPR sympa repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa/).  People willing to also get sympa beta releases should additionally use this [COPR sympa-beta repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa-beta/).

  * Note that [unofficial repository on Sympa-JA.org](http://sympa-ja.org/download/rhel/) is still available.  Administrators who have used it and are not planning to install beta do not need to change their repository settings.

Support for RHEL8/CentOS8 is yet to be decided.

Planned changes in the future
-----------------------------

### Dropping support for Perl 5.8.x

Currently, Sympa officially supports Perl 5.8.1 and better.  However, older versions of Perl have number of flaws while distributors will no longer close them.  That's why we decided to drop support for older versions gradually.

Since Sympa 6.2.46, the next stable release, Perl 5.10.1 and later will be supported.  Nevertheless, administrators are recommended to use as recent version of Perl as they can.

If you have any idea, please comment on the issue [\#620](https://github.com/sympa-community/sympa/issues/620).

### New Sympa logo

New Sympa logo is proposed.  If you have any idea, please comment on the issue [\#665](https://github.com/sympa-community/sympa/issues/665).

----
[The Sympa Community](https://github.com/sympa-community)
