---
assets:
  - created_at: 2019-12-26T04:05:57Z
    name: sympa-6.2.52.tar.gz
    size: 12755075
    updated_at: 2019-12-26T04:08:00Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.52/sympa-6.2.52.tar.gz
  - created_at: 2019-12-26T04:05:57Z
    name: sympa-6.2.52.tar.gz.md5
    size: 54
    updated_at: 2019-12-26T04:08:00Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.52/sympa-6.2.52.tar.gz.md5
  - created_at: 2019-12-26T04:05:58Z
    name: sympa-6.2.52.tar.gz.sha256
    size: 86
    updated_at: 2019-12-26T04:08:01Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.52/sympa-6.2.52.tar.gz.sha256
  - created_at: 2019-12-26T04:05:58Z
    name: sympa-6.2.52.tar.gz.sha512
    size: 158
    updated_at: 2019-12-26T04:08:01Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.52/sympa-6.2.52.tar.gz.sha512
created_at: 2019-12-26T03:58:07Z
prerelease: 0
published_at: 2019-12-27T09:11:46Z
tag_name: 6.2.52
title: Sympa 6.2.52 released
---

<img align="right" src="https://www.sympa.org/_media/logos/old/sympa_multi_150x121.png" title="Sympa logo"/> 27 December 2019

The Sympa Community is proud to release the newest version of Sympa.

**Sympa 6.2.52** is the newest patch release of Sympa 6.2.  **Administrators running or planning to run previous version** (6.2.50) **are strongly recommended to upgrade to this version.**  For more details, see “Note for upgrading” below.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.52/sympa-6.2.52.tar.gz)
  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.52/NEWS.md)
  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)
  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.52/CONTRIBUTING.md)

Note for upgrading
------------------

This version fixes a bug introduced by Sympa 6.2.50 (See issue [#831](https://github.com/sympa-community/sympa/issues/831) for details).  This bug does not affect Sympa 6.2.48 and earlier versions.

Administrators that are running instances which match the following conditions are recommended to upgrade Sympa to this version:

  - Sympa 6.2.50 is running, and
  - there are list(s) either with the ---
      - setting "Who can send messages" (`send`) parameter as "Moderated" (`editorkey`), or
      - enabling "Post" (`compose_mail`) feature on web interface.

If you could not upgrade entire software, [a patch](https://github.com/sympa-community/sympa/compare/6.2.50...11a54e6.diff) including equivalent fixes may be applied.

----
[The Sympa Community](https://github.com/sympa-community)
