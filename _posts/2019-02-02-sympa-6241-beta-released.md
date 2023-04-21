---
assets:
  - created_at: 2019-02-02T14:57:51Z
    name: sympa-6.2.41b.1.tar.gz
    size: 13028170
    updated_at: 2019-02-02T14:59:56Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.41b.1/sympa-6.2.41b.1.tar.gz
  - created_at: 2019-02-02T15:00:16Z
    name: sympa-6.2.41b.1.tar.gz.md5
    size: 57
    updated_at: 2019-02-02T15:00:17Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.41b.1/sympa-6.2.41b.1.tar.gz.md5
  - created_at: 2019-02-02T15:00:27Z
    name: sympa-6.2.41b.1.tar.gz.sha256
    size: 89
    updated_at: 2019-02-02T15:00:28Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.41b.1/sympa-6.2.41b.1.tar.gz.sha256
created_at: 2019-02-02T13:41:54Z
prerelease: 1
published_at: 2019-02-02T15:23:55Z
tag_name: 6.2.41b.1
title: Sympa 6.2.41 beta released
---

<img align="right" src="https://assets.sympa.community/logos/sympa_beta.png" title="Sympa beta logo"/> 2 February 2019

The Sympa Community is proud to release the first beta of the next version of Sympa. Please install it to test and report bugs, translate user interface to your language, or enhance documentation of Sympa, if you want to help the Sympa community deliver a more reliable version of Sympa.

**Sympa 6.2.41b.1** is the new beta version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.41b.1/sympa-6.2.41b.1.tar.gz)

  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.41b.1/NEWS.md)

  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)

  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.41b.1/CONTRIBUTING.md)

This version introduces some improvements and fixes several bugs.

About beta version
---------------------  

This version is a pre-release version of the next stable release.  We expect feedback from users and developers.  To know how to report bugs or improvements, see [Contributing to Sympa](https://github.com/sympa-community/sympa/blob/6.2.41b.1/CONTRIBUTING.md).

  - Next stable release, 6.2.42, is planned to be released on **Wednesday, 20th March 2019** (in UTC).

Translation catalog was updated because some messages were added mainly for new feature.

  - Translations added on [translation site](https://translate.sympa.org/) before 13th March 00:00 UTC will be shipped with the stable release.

Along with several bug fixes, this version introduces several features including following things for beta testing.  

### Global "quiet add" policy (#503)

Have a  [`quiet_subscription`](https://sympa-community.github.io/manual/man/sympa.conf.5.html#quiet_subscription) setting in `sympa.conf` allowing to enforce the "quiet add" policy, either for forcing quiet subscriptions (in enterprise for ex.) or disabling quiet subscriptions (public Sympa server, have to respect GDPR).  Default behavior will be no enforcing, so quiet add is list's owner's choice.

### "Delete my account" button (#300)

A "delete my account" button wll unsubscribe you from all lists and remove your account immediatly.  To protect that feature, Sympa could asks for the user password.  By default this feature is disabled: [`allow_account_deletion`](https://sympa-community.github.io/manual/man/sympa.conf.5.html#allow_account_deletion) setting in `sympa.conf` may activate it.

### "Domain blacklist" feature (#295)

[`domains_blacklist`](https://sympa-community.github.io/manual/man/sympa.conf.5.html#domains_blacklist) setting in `sympa.conf` will prevent addresses with particular domain(s) from subscribing to list.  This feature may be useful for some purposes, for example not to register addresses with domains no longer providing mail service.

Packaging
---------

### Debian

The freeze for the next Debian release has been started and will culminate in a [full freeze on 2019-03-12](https://release.debian.org/buster/freeze_policy.html).

The latest stable release of Sympa (6.2.40) has been migrated to Debian testing. If possible, more improvements may be added before the doors are closed.  In addition to that, it is planned that the latest release will be got into stretch backports.

To help [Debian Sympa Team](https://qa.debian.org/developer.php?login=sympa%40packages.debian.org) with testing the packages, please contact them.

### RPM

People running either Fedora or RHEL (and clones) can find ready to use RPMs for sympa beta releases in a [COPR sympa-beta repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa-beta/).

For Fedora, see instruction in the page above.  For RHEL and clones, you also need to enable the [EPEL repository](https://www.fedoraproject.org/wiki/EPEL).

  * Note that [unofficial repository on Sympa-JA.org](http://sympa-ja.org/download/rhel/) is still available.  Administrators who have used it and are not planning to install beta do not need to change their repository settings.

----
[The Sympa Community](https://github.com/sympa-community)
