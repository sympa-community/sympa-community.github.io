---
assets:
  - created_at: 2019-01-18T11:57:20Z
    name: sympa-6.2.40.tar.gz
    size: 12974706
    updated_at: 2019-01-18T11:57:28Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.40/sympa-6.2.40.tar.gz
  - created_at: 2019-01-18T11:57:35Z
    name: sympa-6.2.40.tar.gz.md5
    size: 54
    updated_at: 2019-01-18T11:57:37Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.40/sympa-6.2.40.tar.gz.md5
  - created_at: 2019-01-18T11:57:42Z
    name: sympa-6.2.40.tar.gz.sha256
    size: 86
    updated_at: 2019-01-18T11:57:42Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.40/sympa-6.2.40.tar.gz.sha256
created_at: 2019-01-18T10:42:07Z
prerelease: 0
published_at: 2019-01-19T07:57:59Z
tag_name: 6.2.40
title: Sympa 6.2.40 released
---

<img align="right" src="https://www.sympa.org/_media/logos/old/sympa_multi_150x121.png" title="Sympa logo"/> 19 January 2019

The Sympa Community is proud to release the newest version of Sympa.

**Sympa 6.2.40** is the newest patch release of Sympa 6.2.  **Administrators running or planning to run previous version** (6.2.38) **are strongly recommended to upgrade to this version.**  For more details, see “Note for upgrading” below.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.40/sympa-6.2.40.tar.gz)
  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.40/NEWS.md)
  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)
  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.40/CONTRIBUTING.md)

Note for upgrading
------------------

This version fixes a bug introduced into Sympa 6.2.38 (See issue [#527](https://github.com/sympa-community/sympa/issues/527) for details).  This bug does not affect earlier versions.

Administrators matching following conditions are recommended to upgrade Sympa to this version.

  - Sympa 6.2.38 is running,
  - web interface is available, and
  - there are lists which either:
      - make archives open to public and enable "cookie" spam protection (note that "cookie" setting is enabled by default),
      - allow the users to unsubscribe themselves from the list using web interface (including use of "auto_signoff" link in message footer), or
      - allow to use URL links in system messages so that users authorize requests for adding, removing, moving or reminding users.

----
[The Sympa Community](https://github.com/sympa-community)
