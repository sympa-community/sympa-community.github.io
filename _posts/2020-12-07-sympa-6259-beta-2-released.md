---
assets:
  - created_at: 2020-12-07T08:20:05Z
    name: sympa-6.2.59b.2.tar.gz
    size: 12872213
    updated_at: 2020-12-07T08:20:08Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.59b.2/sympa-6.2.59b.2.tar.gz
  - created_at: 2020-12-07T08:20:08Z
    name: sympa-6.2.59b.2.tar.gz.md5
    size: 57
    updated_at: 2020-12-07T08:20:08Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.59b.2/sympa-6.2.59b.2.tar.gz.md5
  - created_at: 2020-12-07T08:20:08Z
    name: sympa-6.2.59b.2.tar.gz.sha256
    size: 89
    updated_at: 2020-12-07T08:20:09Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.59b.2/sympa-6.2.59b.2.tar.gz.sha256
  - created_at: 2020-12-07T08:20:09Z
    name: sympa-6.2.59b.2.tar.gz.sha512
    size: 161
    updated_at: 2020-12-07T08:20:09Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.59b.2/sympa-6.2.59b.2.tar.gz.sha512
created_at: 2020-12-07T08:05:25Z
prerelease: 1
published_at: 2020-12-07T08:20:54Z
tag_name: 6.2.59b.2
title: Sympa 6.2.59 beta 2 released
---

<img align="right" src="https://www.sympa.org/_media/logos/old/sympa_beta.png" title="Sympa beta logo"/> 7 December 2020

The Sympa Community is proud to release the new beta of the next version of Sympa. Please install it to test and report bugs, translate user interface to your language, or enhance documentation of Sympa, if you want to help the Sympa community deliver a more reliable version of Sympa.

**Sympa 6.2.59b.2** is the new beta version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.59b.2/sympa-6.2.59b.2.tar.gz)

  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.59b.2/NEWS.md)

  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)

  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.59b.2/CONTRIBUTING.md)

This version introduces some improvements and fixes several bugs.  See also "Significant changes" below.

About beta version
---------------------  

This version is a pre-release version of the next stable release.  We expect feedback from users and developers.  To know how to report bugs or improvements, see [Contributing to Sympa](https://github.com/sympa-community/sympa/blob/6.2.59b.2/CONTRIBUTING.md).

  - Next stable release, 6.2.60, is planned to be released on **Monday, 21st December 2020** (in UTC).

Translation catalog was updated because some messages were added mainly for new features.

  - Translations added on [translation site](https://translate.sympa.org/) before 19th December 00:00 UTC will be shipped with the stable release.

### Significant changes

  * Several options at installation and run time to get rid of setuid wrappers were introduced.  These changes may help avoiding potential risk by using setuid programs.
      - Now the setuid wrappers may be disabled, if installation process allows. Packagers are encouraged to provide configuration not using setuid wrappers as possible.  See also [\#943](https://github.com/sympa-community/sympa/issues/943) and related issues/PRs.
  * Personalization (formerly sometimes called "merge feature") is now restricted by default: It is enabled only when the message is posted via web interface, and is applied only on footer and header (if any).
    This behavior may be changed using `personalization` list parameter, however, listmasters are recommended to review whether wide range of conversion as previous versions is required.  See also [\#1037](https://github.com/sympa-community/sympa/issues/1037).

Related project
---------------

### MHonArc will be maintained by us

Now The Sympa Community is in charge of maintenance of [MHonArc](https://www.mhonarc.org/), a mail-to-HTML converter.

  * Repository is [``sympa-community/MHonArc`` on GitHub](https://github.com/sympa-community/MHonArc).
  * Issues are tracked on [Issues on GitHub](https://github.com/sympa-community/MHonArc/issues).
  * The latest version will be released on [MetaCPAN](https://metacpan.org/release/MHonArc) as before.

----
[The Sympa Community](https://github.com/sympa-community)
