---
assets:
  - created_at: 2017-09-29T22:23:39Z
    name: sympa-6.2.22.tar.gz
    size: 12469435
    updated_at: 2017-09-29T22:24:27Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.22/sympa-6.2.22.tar.gz
created_at: 2017-09-29T21:49:26Z
prerelease: 0
published_at: 2017-10-01T06:11:32Z
tag_name: 6.2.22
title: Sympa 6.2.22 released
---

<img align="right" src="https://assets.sympa.community/logos/sympa_multi_150x121.png" title="Sympa logo"/> 1 October 2017

The Sympa Community is proud to release the newest  version of Sympa.

**Sympa 6.2.22** is the newest patch release of Sympa 6.2.  See “Note for upgrading” to decide if you should upgrade.

- [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.22/sympa-6.2.22.tar.gz)
- [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.22/NEWS.md)
- [Check the upgrade instructions from earlier versions](https://www.sympa.org/faq/upgrade-to-v6.2)

- [Let's contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.22/CONTRIBUTING.md)

Note for upgrading
------------------

This version fixes following critical bugs.
- upgrade_send_spool.pl could leave some messages not upgraded \[[diff](https://github.com/sympa-community/sympa/compare/6.2.20...ce1f94d239062b845524de60a84a711af29d1ec6)\]
- "sympa.pl --change_user_email" was broken [#65](https://github.com/sympa-community/sympa/pull/65)

Administrators in either of conditions below are recommended to upgrade Sympa to this version.
- Sympa prior to 6.2 has been installed, and waits for upgrading.
- Sympa 6.2.20 has been installed, and `sympa.pl` can't be invoked with `--change_user_email` option.

----
[The Sympa Community](https://github.com/sympa-community)
