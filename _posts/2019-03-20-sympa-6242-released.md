---
assets:
  - created_at: 2019-03-20T13:35:11Z
    name: sympa-6.2.42.tar.gz
    size: 13195688
    updated_at: 2019-03-20T13:37:19Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.42/sympa-6.2.42.tar.gz
  - created_at: 2019-03-20T13:37:41Z
    name: sympa-6.2.42.tar.gz.md5
    size: 54
    updated_at: 2019-03-20T13:37:43Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.42/sympa-6.2.42.tar.gz.md5
  - created_at: 2019-03-20T13:37:51Z
    name: sympa-6.2.42.tar.gz.sha256
    size: 86
    updated_at: 2019-03-20T13:37:52Z
    url: https://github.com/sympa-community/sympa/releases/download/6.2.42/sympa-6.2.42.tar.gz.sha256
created_at: 2019-03-20T12:52:07Z
prerelease: 0
published_at: 2019-03-20T13:52:42Z
tag_name: 6.2.42
title: Sympa 6.2.42 released
---

<img align="right" src="https://assets.sympa.community/logos/sympa_multi_150x121.png" title="Sympa logo"/> 20 March 2019

The Sympa Community is proud to release the newest version of Sympa.

**Sympa 6.2.42** is the newest stable version of Sympa 6.2.

  - [Download source distribution](https://github.com/sympa-community/sympa/releases/download/6.2.42/sympa-6.2.42.tar.gz)
  - [Check the release notes](https://github.com/sympa-community/sympa/blob/6.2.42/NEWS.md)
  - [Check the upgrade instructions from earlier versions](https://sympa-community.github.io/manual/upgrade/notes.html)
  - [How to contribute to Sympa](https://github.com/sympa-community/sympa/blob/6.2.42/CONTRIBUTING.md)

This version fixes several bugs, and introduces some enhancements including new features described in below.  Translations to several languages have been mostly completed.  Administrators are encouraged to upgrade Sympa to this version.

Highlight of this version
-------------------------

### New features

This version introduces several enhancements.  Among them, following features are contributed by Luc Didry, Framasoft.

  - Global "quiet add" policy (#503)
    Have a  [`quiet_subscription`](https://sympa-community.github.io/manual/man/sympa.conf.5.html#quiet_subscription) setting in `sympa.conf` allowing to enforce the "quiet add" policy, either for forcing quiet subscriptions (in enterprise for ex.) or disabling quiet subscriptions (public Sympa server, have to respect GDPR).  Default behavior will be no enforcing, so quiet add is list's owner's choice.

  - "Delete my account" button (#300)
    A "delete my account" button wll unsubscribe you from all lists and remove your account immediately.  To protect that feature, Sympa could asks for the user password.  By default this feature is disabled: [`allow_account_deletion`](https://sympa-community.github.io/manual/man/sympa.conf.5.html#allow_account_deletion) setting in `sympa.conf` may activate it.

  - "Domain blacklist" feature (#295)
    [`domains_blacklist`](https://sympa-community.github.io/manual/man/sympa.conf.5.html#domains_blacklist) setting in `sympa.conf` will prevent addresses with particular domain(s) from subscribing to list.  This feature may be useful for some purposes, for example not to register addresses with domains no longer providing mail service.

### Internationalization

Thanks to heavy works by translators on [translation site](https://translate.sympa.org), Sympa almost completely supports following languages:

  * Italian (Italiano)
  * Spanish (Español)
  * Galician (Galego)
  * German (Deutsch)
  * French (Français)
  * Russian (Русский)
  * Japanese (日本語)
  * US English

Along with languages above, help documents for users are provided in following languages:

  * Catalan (Català)
  * Basque (Euskara)
  * Polish (Polski)

----
[The Sympa Community](https://github.com/sympa-community)
