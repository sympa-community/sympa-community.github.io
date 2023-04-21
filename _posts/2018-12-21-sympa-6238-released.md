---
assets:
  - created_at: 2018-12-21T02:35:46Z
    name: sympa-6.2.38.tar.gz
    size: 12975608
    updated_at: 2018-12-21T02:37:49Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.38/sympa-6.2.38.tar.gz
  - created_at: 2018-12-21T02:39:10Z
    name: sympa-6.2.38.tar.gz.md5
    size: 54
    updated_at: 2018-12-21T02:39:11Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.38/sympa-6.2.38.tar.gz.md5
  - created_at: 2018-12-21T02:39:23Z
    name: sympa-6.2.38.tar.gz.sha256
    size: 86
    updated_at: 2018-12-21T02:39:27Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.38/sympa-6.2.38.tar.gz.sha256
created_at: 2018-12-21T02:36:47Z
prerelease: 0
published_at: 2018-12-21T12:10:42Z
tag_name: 6.2.38
title: Sympa 6.2.38 released
---

<img align="right" src="https://assets.sympa.community/logos/sympa_multi_150x121.png" title="Sympa logo"/> 21 December 2018

The Sympa Community is proud to release the newest version of Sympa.

**Sympa 6.2.38** is the newest stable version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.38/sympa-6.2.38.tar.gz)
  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.38/NEWS.md)
  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)
  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.38/CONTRIBUTING.md)

This version fixes several bugs, and introduces some enhancements including new features described in below.  Translations to several languages have been mostly completed.  Administrators are encouraged to upgrade Sympa to this version.

Highlight of this version
-------------------------

### ARC support

Sympa supports ARC (Authenticated Received Chain).  Its implementation and documentation were contributed by John Levine.

In a nutshell, ARC is an e-mail authentication scheme that remedies a problem of DMARC especially with mailing list service. About ARC in general, see a [slide by DMARC.org](https://dmarc.org/presentations/ARC-Overview-2016Q3-v01.pdf) (PDF). To enable ARC on Sympa, [documentation on DKIM and ARC with Sympa](https://sympa-community.github.io/manual/customize/dkim-arc.html).

### CSRF tolerance

Random token to prevent CSRF (Cross-Site Request Forgeries) attacks will be inserted in every form on web interface. This enhancement was contributed by Michael Kaczmarczik, UT Austin.

### Internationalization

Thanks to heavy works by translators on [translation site](https://translate.sympa.org), Sympa completely or almost completely supports following languages:

  * Spanish (Español)
  * Galician (Galego)
  * German (Deutsch)
  * French (Français)
  * Russian (Русский)
  * Japanese (日本語)
  * US English

Along with languages above, help documents for users are provided in following languages:

  * Catalan (Català)
  * Basque (Euskara)
  * Polish (Polski)

### Binary distribution for Fedora and RHEL (and clones)

People running either Fedora or RHEL (and clones) can find ready to use RPMs for sympa in a [COPR sympa repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa/).
People willing to also get sympa beta releases should additionally use this [COPR sympa-beta repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa-beta/).

For Fedora, see instruction in the pages above.  For RHEL and clones, you also need to enable the [EPEL repository](https://www.fedoraproject.org/wiki/EPEL).

  * Note that [unofficial repository on Sympa-JA.org](http://sympa-ja.org/download/rhel/) is still available.  Administrators who have used it and are not planning to install beta do not need to change their repository settings.

----
Have a great new year with Sympa!

[The Sympa Community](https://github.com/sympa-community)
