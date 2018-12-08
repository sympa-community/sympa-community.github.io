---
assets:
  - created_at: 2018-12-08T12:55:42Z
    name: sympa-6.2.37b.3.tar.gz
    size: 13349949
    updated_at: 2018-12-08T12:57:51Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.37b.3/sympa-6.2.37b.3.tar.gz
  - created_at: 2018-12-08T12:58:07Z
    name: sympa-6.2.37b.3.tar.gz.md5
    size: 57
    updated_at: 2018-12-08T12:58:08Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.37b.3/sympa-6.2.37b.3.tar.gz.md5
  - created_at: 2018-12-08T12:58:16Z
    name: sympa-6.2.37b.3.tar.gz.sha256
    size: 89
    updated_at: 2018-12-08T12:58:17Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.37b.3/sympa-6.2.37b.3.tar.gz.sha256
created_at: 2018-12-08T11:55:28Z
prerelease: 1
published_at: 2018-12-08T13:29:54Z
tag_name: 6.2.37b.3
title: Sympa 6.2.37 beta 3 released
---

<img align="right" src="https://www.sympa.org/_media/logos/old/sympa_beta.png" title="Sympa beta logo"/> 8 Dec 2018

The Sympa Community is proud to release the third beta of the next version of Sympa. Please install it to test and report bugs, translate user interface to your language, or enhance documentation of Sympa, if you want to help the Sympa community deliver a more reliable version of Sympa.

**Sympa 6.2.37b.3** is the new beta version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.37b.3/sympa-6.2.37b.3.tar.gz)

  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.37b.3/NEWS.md)

  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)

  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.37b.3/CONTRIBUTING.md)

This version introduces some improvements and fixes several bugs.

About beta version
---------------------

This version is a pre-release version of the next stable release.  We expect feedback from users and developers.  To know how to report bugs or improvements, see [Contributing to Sympa](https://github.com/sympa-community/sympa/blob/6.2.37b.3/CONTRIBUTING.md).

  - Next stable release, 6.2.38, is planned to be released on **Friday, 21st December 2018**.

Translation catalog was updated because some messages were added mainly for new feature.

  - Translations added on [translation site](https://translate.sympa.org/) before 19th December 00:00 UTC will be shipped with the stable release.

### Binary distribution for Fedora and RHEL (and clones)

People running either Fedora or RHEL (and clones) can find ready to use RPMs for sympa 6.2.37 beta 3 in a [COPR repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa/).

For Fedora, see instruction in the page above.  For RHEL and clones, you also need to enable the [EPEL repository](https://www.fedoraproject.org/wiki/EPEL).

  * Note that [unofficial repository on Sympa-JA.org](http://sympa-ja.org/download/rhel/) is still available.  Administrators who have used it and are not planning to install beta do not need to change their repository settings.

Adding ARC support
------------------

This version also aims to be a draft of ARC (Authenticated Received Chain) support with Sympa.  Its implementation and documentation were contributed by John Levine.
  - [Issue #153 on GitHub](https://github.com/sympa-community/sympa/issues/153)
  - [Draft documentation on DKIM and ARC with Sympa](https://sympa-community.github.io/manual/customize/dkim-arc.html)

In a nutshell, ARC is an e-mail authentication scheme that remedies a problem of DMARC especially with mailing list service.  Supporting it, we may avoid confusing rewriting on originator addresses.  For details about ARC, see a [slide by DMARC.org](https://dmarc.org/presentations/ARC-Overview-2016Q3-v01.pdf) (PDF).

We are looking forward to your feedback on [GitHub issue page](https://github.com/sympa-community/sympa/issues/153) or [mailing lists](https://sympa-community.github.io/community/lists.html).

----
[The Sympa Community](https://github.com/sympa-community)
