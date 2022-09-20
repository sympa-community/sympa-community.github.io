---
title: 'DKIM and ARC: Setup MTA: Using OpenDMARC'
up: dkim-arc-setup-mta.md
prev: dkim-arc-setup-keys.md
next: dkim-arc-setup-sympa.md
---

DKIM and ARC: Setup MTA: Using OpenDMARC
========================================

Requirements
------------

  * MTA: Postfix or Sendmail
  * [OpenDKIM](http://www.opendkim.org/)
  * [OpenDMARC](http://www.trusteddomain.org/opendmarc/)

  * You have to choose **authserv-id** to determine the results of domain
    validation.
    In this document `mx.example.org` is used for example.

----
Note:

  * It is assumed here that OpenDMARC was built with internal SPF checking
    enabled. If not, you will need to install additional software for SPF,
    such as:

      - [pypolicyd-spf](https://launchpad.net/pypolicyd-spf)
        (for Postfix only)
      - [SPF Engine](https://launchpad.net/spf-engine)
      - [SPF Milter](https://crates.io/crates/spf-milter)
      - [spfmilter](https://github.com/sdgathman/milter/#spfmilter)

----

Configuration
-------------

### Setting OpenDKIM / OpenDMARC

Sympa is responsible for the DKIM signing. That is, on OpenDKIM, the value
of `Mode` parameter below may only include `v`, for verification.

The minimum configuration is as follows.

`opendkim.conf` (see the
[manual of OpenDKIM](http://www.opendkim.org/opendkim.conf.5.html)
for details):
``` code
AlwaysAddARHeader yes
AuthservID mx.example.org
Mode v
Socket inet:8891@localhost
```

`opendmarc.conf` (see the
[manual of OpenDMARC](http://www.trusteddomain.org/opendmarc/opendmarc.conf.5.html)
for details):
``` code
AuthservID mx.example.org
TrustedAuthservIDs mx.example.org
Socket inet:8893@localhost
SPFSelfValidate true
```

### Setting MTA

  * Postfix

    Add following settings to `main.cf`:

    ``` code
    smtpd_milters = (existing settings) inet:localhost:8891 inet:localhost:8893
    milter_default_action = accept
    ```

  * Sendmail

    Edit `sendmai.cf` to add following settings:
    ``` code
    O InputMailFilters=opendkim, opendmarc
    Xopendkim, S=inet:8891@localhost
    Xopendmarc, S=inet:8893@localhost
    ```
    Or, if you are generating `sendmail.cf` from `sendmail.mc`, add following
    lines after `FEATURE` lines:
    ``` code
    define(`confINPUT_MAIL_FILTERS', `opendkim, opendmarc')
    MAIL_FILTER(`opendkim', `S=inet:8891@localhost')
    MAIL_FILTER(`opendmarc', `S=inet:8893@localhost')
    ```

