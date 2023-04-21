---
assets:
  - created_at: 2021-01-06T08:29:59Z
    name: sympa-6.1.25-sa-2020-003-r1.patch
    size: 1098
    updated_at: 2021-01-06T08:30:00Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.60/sympa-6.1.25-sa-2020-003-r1.patch
  - created_at: 2021-01-06T08:29:37Z
    name: sympa-6.2.24-sa-2020-003-r1.patch
    size: 1131
    updated_at: 2021-01-06T08:29:38Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.60/sympa-6.2.24-sa-2020-003-r1.patch
  - created_at: 2021-01-04T07:06:39Z
    name: sympa-6.2.58-sa-2020-003-r1.patch
    size: 1510
    updated_at: 2021-01-04T07:06:40Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.60/sympa-6.2.58-sa-2020-003-r1.patch
  - created_at: 2021-01-04T07:40:49Z
    name: sympa-6.2.60.tar.gz
    size: 12869091
    updated_at: 2021-01-04T07:40:52Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.60/sympa-6.2.60.tar.gz
  - created_at: 2021-01-04T07:40:52Z
    name: sympa-6.2.60.tar.gz.md5
    size: 54
    updated_at: 2021-01-04T07:40:52Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.60/sympa-6.2.60.tar.gz.md5
  - created_at: 2021-01-04T07:40:52Z
    name: sympa-6.2.60.tar.gz.sha256
    size: 86
    updated_at: 2021-01-04T07:40:53Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.60/sympa-6.2.60.tar.gz.sha256
  - created_at: 2021-01-04T07:40:53Z
    name: sympa-6.2.60.tar.gz.sha512
    size: 158
    updated_at: 2021-01-04T07:40:53Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.60/sympa-6.2.60.tar.gz.sha512
created_at: 2021-01-04T07:18:24Z
prerelease: 0
published_at: 2021-01-04T07:50:58Z
tag_name: 6.2.60
title: Sympa 6.2.60 released
---

<img align="right" src="https://assets.sympa.community/logos/sympa_multi_150x121.png" title="Sympa logo"/> 4 January 2021

The Sympa Community is proud to release the newest version of Sympa.

**Sympa 6.2.60** is the newest stable version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.60/sympa-6.2.60.tar.gz)
  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.60/NEWS.md)
  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)
  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.60/CONTRIBUTING.md)

This version fixes several bugs and introduces some enhancements.  Especially, it includes _the fix for security flaw_ (see below).  Translations to several languages have been mostly completed.  Administrators are encouraged to upgrade Sympa to this version.

Highlights of this version
--------------------------

### Security flaw found in previous versions (CVE-2020-29668)

Administrators that are running instances which match the following conditions are strongly recommended to upgrade Sympa to this version:

  * Sympa 6.2.58 or earlier is installed,

  * SOAP/HTTP interface (``sympa_soap_server.fcgi``) is running, and

  * ``authenticateAndRun`` call can be accessed by any SOAP clients.

See also [Security Advisory](https://sympa-community.github.io/security/2020-003.html) for details.


### Some features may be restricted by default

  * The behaviour of the personalization / merge feature has been changed and may affect the processing of your emails. We strongly recommend to read the [Upgrading notes](https://sympa-community.github.io/manual/upgrade/notes.html#from-version-prior-to-6260) if you have this feature enabled globally or for some lists.

  * Several options at installation and run time to get rid of setuid wrappers were introduced.  See [Upgrading notes](https://sympa-community.github.io/manual/upgrade/notes.html#from-version-prior-to-6260) for details.

### Bug fixes and improvements

Check [the release notes](https://github.com/sympa-community/sympa/blob/6.2.60/NEWS.md) for details.

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

Related project
---------------

### MHonArc will be maintained by us

Now The Sympa Community is in charge of maintenance of [MHonArc](https://www.mhonarc.org/), a mail-to-HTML converter.

  * Repository is [``sympa-community/MHonArc`` on GitHub](https://github.com/sympa-community/MHonArc).
  * Issues are tracked on [Issues on GitHub](https://github.com/sympa-community/MHonArc/issues).
  * The latest version will be released on [MetaCPAN](https://metacpan.org/release/MHonArc) as before.

----
Have a great new year with Sympa!

[The Sympa Community](https://github.com/sympa-community)
