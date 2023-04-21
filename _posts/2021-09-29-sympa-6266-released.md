---
assets:
  - created_at: 2021-09-29T04:42:55Z
    name: sympa-6.2.66.tar.gz
    size: 12955367
    updated_at: 2021-09-29T04:43:02Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.66/sympa-6.2.66.tar.gz
  - created_at: 2021-09-29T04:43:02Z
    name: sympa-6.2.66.tar.gz.md5
    size: 54
    updated_at: 2021-09-29T04:43:02Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.66/sympa-6.2.66.tar.gz.md5
  - created_at: 2021-09-29T04:42:50Z
    name: sympa-6.2.66.tar.gz.sha256
    size: 86
    updated_at: 2021-09-29T04:42:55Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.66/sympa-6.2.66.tar.gz.sha256
  - created_at: 2021-09-29T04:42:54Z
    name: sympa-6.2.66.tar.gz.sha512
    size: 158
    updated_at: 2021-09-29T04:42:56Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.66/sympa-6.2.66.tar.gz.sha512
created_at: 2021-09-29T04:32:48Z
prerelease: 0
published_at: 2021-09-29T04:50:21Z
tag_name: 6.2.66
title: Sympa 6.2.66 released
---

<img align="right" src="https://assets.sympa.community/logos/sympa_multi_150x121.png" title="Sympa logo"/> 29th September 2021

The Sympa Community is proud to release the newest version of Sympa.

**Sympa 6.2.66** is the newest stable version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.66/sympa-6.2.66.tar.gz)
  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.66/NEWS.md)
  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)
  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.66/CONTRIBUTING.md)

This version fixes several bugs and introduces some enhancements.  Translations to several languages have been mostly completed.  Administrators are encouraged to upgrade Sympa to this version.

Highlights of this version
--------------------------

  * Improving data source synchronization performance ([\#1186](https://github.com/sympa-community/sympa/issues/1186))

    The `sync_include` process (synchronization of list users with data sources) was optimized. Some benchmarks show a 3x reduction of processing time.

  * Now the characters used for e-mail addresses conform to RFC 5322 ([#1217](https://github.com/sympa-community/sympa/issues/1217))

    Some characters such as backtick are allowed in the e-mail addresses of the users.

For details on the other changes see [the release notes](https://github.com/sympa-community/sympa/blob/6.2.66/NEWS.md).

### Internationalization

Thanks to heavy works by translators on [translation site](https://translate.sympa.org), Sympa almost completely supports following languages:

  * Japanese (日本語)
  * Russian (Русский)
  * Galician (Galego)
  * Spanish (Español)
  * German (Deutsch)
  * French (Français)
  * Italian (Italiano)
  * US English

Along with languages above, help documents for users are provided in following languages:

  * Catalan (Català)
  * Basque (Euskara)
  * Polish (Polski)

----

[The Sympa Community](https://github.com/sympa-community)
