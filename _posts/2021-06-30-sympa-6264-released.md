---
assets:
  - created_at: 2021-06-30T08:04:19Z
    name: sympa-6.2.64.tar.gz
    size: 12919309
    updated_at: 2021-06-30T08:04:47Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.64/sympa-6.2.64.tar.gz
  - created_at: 2021-06-30T08:04:16Z
    name: sympa-6.2.64.tar.gz.md5
    size: 54
    updated_at: 2021-06-30T08:04:17Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.64/sympa-6.2.64.tar.gz.md5
  - created_at: 2021-06-30T08:04:17Z
    name: sympa-6.2.64.tar.gz.sha256
    size: 86
    updated_at: 2021-06-30T08:04:18Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.64/sympa-6.2.64.tar.gz.sha256
  - created_at: 2021-06-30T08:04:18Z
    name: sympa-6.2.64.tar.gz.sha512
    size: 158
    updated_at: 2021-06-30T08:04:19Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.64/sympa-6.2.64.tar.gz.sha512
created_at: 2021-06-30T07:51:50Z
prerelease: 0
published_at: 2021-06-30T09:32:33Z
tag_name: 6.2.64
title: Sympa 6.2.64 released
---

<img align="right" src="https://www.sympa.org/_media/logos/old/sympa_multi_150x121.png" title="Sympa logo"/> 30th June 2021

The Sympa Community is proud to release the newest version of Sympa.

**Sympa 6.2.64** is the newest stable version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.64/sympa-6.2.64.tar.gz)
  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.64/NEWS.md)
  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)
  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.64/CONTRIBUTING.md)

This version fixes several bugs and introduces some enhancements.  Translations to several languages have been mostly completed.  Administrators are encouraged to upgrade Sympa to this version.

Highlights of this version
--------------------------

Improvements:

- New scenarios: List visibility scenario `visibility.identified` for logged in users [\#1140](https://github.com/sympa-community/sympa/pull/1140), and the scenario `create_list.closed` to disallow list creation [\#1191](https://github.com/sympa-community/sympa/pull/1191).
- Ability to forbid some list names with `prohibited_listnames` and `prohibited_listnames_regex` parameters [\#672](https://github.com/sympa-community/sympa/issues/672).

Bug fixes:

- S/MIME: Extracting certificate with multiple email values fails [\#1196](https://github.com/sympa-community/sympa/issues/1196).

Many more bug fixes and improvements have been done on this release.  Check [the release notes](https://github.com/sympa-community/sympa/blob/6.2.64/NEWS.md) for more details.

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
