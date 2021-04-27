---
assets:
  - created_at: 2021-04-27T01:54:36Z
    name: sympa-6.2.62.tar.gz
    size: 12904131
    updated_at: 2021-04-27T01:54:59Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.62/sympa-6.2.62.tar.gz
  - created_at: 2021-04-27T01:54:59Z
    name: sympa-6.2.62.tar.gz.md5
    size: 54
    updated_at: 2021-04-27T01:54:59Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.62/sympa-6.2.62.tar.gz.md5
  - created_at: 2021-04-27T01:54:59Z
    name: sympa-6.2.62.tar.gz.sha256
    size: 86
    updated_at: 2021-04-27T01:55:00Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.62/sympa-6.2.62.tar.gz.sha256
  - created_at: 2021-04-27T01:55:00Z
    name: sympa-6.2.62.tar.gz.sha512
    size: 158
    updated_at: 2021-04-27T01:55:00Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.62/sympa-6.2.62.tar.gz.sha512
created_at: 2021-04-27T01:46:13Z
prerelease: 0
published_at: 2021-04-27T06:04:55Z
tag_name: 6.2.62
title: Sympa 6.2.62 released
---

<img align="right" src="https://www.sympa.org/_media/logos/old/sympa_multi_150x121.png" title="Sympa logo"/> 27th April 2021

The Sympa Community is proud to release the newest version of Sympa.

**Sympa 6.2.62** is the newest stable version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.62/sympa-6.2.62.tar.gz)
  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.62/NEWS.md)
  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)
  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.62/CONTRIBUTING.md)

This version fixes several bugs and introduces some enhancements.  Translations to several languages have been mostly completed.  Administrators are encouraged to upgrade Sympa to this version.

Highlights of this version
--------------------------

- `cookie` parameter in `sympa.conf` was obsoleted [\#1091](https://github.com/sympa-community/sympa/issues/1091).  For more details read [Security Advisory 2021-001](https://sympa-community.github.io/security/2021-001.html).
- By a request from the user with English background, the term "blacklist" was replaced with "blocklist".  By this change, names of some parameters and configuration file were changed [\#1111](https://github.com/sympa-community/sympa/issues/1111) [\#1144](https://github.com/sympa-community/sympa/issues/1144). This change will be migrated automatically during upgrading process.

About the other bug fixes and improvements, check [the release notes](https://github.com/sympa-community/sympa/blob/6.2.62/NEWS.md) for details.

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
