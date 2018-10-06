---
assets:
  - created_at: 2018-10-06T11:13:10Z
    name: sympa-6.2.37b.1.tar.gz
    size: 12908686
    updated_at: 2018-10-06T11:15:16Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.37b.1/sympa-6.2.37b.1.tar.gz
  - created_at: 2018-10-06T11:15:28Z
    name: sympa-6.2.37b.1.tar.gz.md5
    size: 57
    updated_at: 2018-10-06T11:15:29Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.37b.1/sympa-6.2.37b.1.tar.gz.md5
  - created_at: 2018-10-06T11:15:38Z
    name: sympa-6.2.37b.1.tar.gz.sha256
    size: 89
    updated_at: 2018-10-06T11:15:39Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.37b.1/sympa-6.2.37b.1.tar.gz.sha256
created_at: 2018-10-06T10:03:43Z
prerelease: 1
published_at: 2018-10-06T11:15:46Z
tag_name: 6.2.37b.1
title: Sympa 6.2.37 beta released
---

<img align="right" src="https://www.sympa.org/_media/logos/old/sympa_beta.png" title="Sympa beta logo"/> 6 Oct 2018

The Sympa Community is proud to release the first beta of the next version of Sympa. Please install it to test and report bugs, translate user interface to your language, or enhance documentation on Sympa, if you want to help the Sympa community to deliver a more reliable version of Sympa.

**Sympa 6.2.37b.1** is the new beta version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.37b.1/sympa-6.2.37b.1.tar.gz)

  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.37b.1/NEWS.md)

  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)

  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.37b.1/CONTRIBUTING.md)

This version introduces some inprovements including ARC support (see below) and fixes a few bugs.

About beta version
---------------------

This version is a prerelease version of new stable release.  We expect feedback from users and developers.  To know how to report bugs or improvements, see [Contributing to Sympa](https://github.com/sympa-community/sympa/blob/6.2.37b.1/CONTRIBUTING.md).

  - New stable release is planned to be released in **Friday, 21st December 2018**.

Translation catalog was updated because some messages were added mainly for new feature.

  - Translations added on [translation site](https://translate.sympa.org/) not after 19th December 00:00 UTC will be shipped with new stable release.

Adding ARC support
------------------

This version aims to be the first draft of ARC (Authenticated Received Chain) support with Sympa.  For details about ARC, see a [slide by DMARC.org](https://dmarc.org/presentations/ARC-Overview-2016Q3-v01.pdf) (PDF).

In a nutshell, ARC is an e-mail authentication scheme that remedies a problem of DMARC especially with mailing list service.  Supporting it, we may avoid confusing rewriting on originator addresses.

We will look forward your feedbacks on:
  - GitHub issue page: [Issue #153](https://github.com/sympa-community/sympa/issues/153)
  - Mailing lists: [sympa-developpers, sympa-users etc.](https://sympa-community.github.io/community/lists.html)

----
[The Sympa Community](https://github.com/sympa-community)
