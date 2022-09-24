---
title: 'DKIM and ARC: Setup MTA: Using Rspamd'
up: dkim-arc-setup-mta.md
prev: dkim-arc-setup-keys.md
next: dkim-arc-setup-mta.md#tests
---

DKIM and ARC: Setup MTA: Using Rspamd
=====================================

Requirements
------------

  * MTA: Postfix, Sendmail, or maybe OpenSMTPD.
  * [Rspamd](https://www.rspamd.com/) 3.3 STABLE or later.

    ----
    Note:

      * While Rspamd 3.3 STABLE has not been released, HEAD of master
        branch in the
        [source repository](https://github.com/rspamd/rspamd) may be
        used instead.
        Earlier version of Rspamd does not work as expected with Sympa.

    ----

  * You have to choose (or confirm) **authserv-id** to determine the
    results of domain validation.  See descriptions below for details.
    In this document `mx.example.org` is used for example.

Configuration
-------------

### Setting Rspamd

First, integrate MTA with Rspamd according to the instruction
"[MTA integration](https://rspamd.com/doc/integration.html)".

Note that,
Rspamd also has DKIM signing and ARC sealing capabilities, but we may
not configure them: Sympa is responsible for those features.

Thus, basically the configuration of Rspamd we have to do for Sympa is just
adding following line to `local.d/milter_headers.conf`:

``` code
use = ["authentication-results"];
```
For details see the
[documentation](https://rspamd.com/doc/modules/milter_headers.html).

### Setting MTA

  * Postfix

    Add following settings to `main.cf` (Note:
    Replace `mx.example.org` in below):

    ``` code
    smtpd_milters = (existing settings) inet:localhost:11332
    milter_default_action = accept
    milter_macro_daemon_name = mx.example.org
    ```
    If `milter_macro_daemon_name` is not specified, value of `myhostname`
    is used as authserv-id.

  * Sendmail

    Edit `sendmail.cf` to add following settings:
    ``` code
    O InputMailFilters=rspamd
    Xrspamd, S=inet:11332@localhost
    ```
    Or, if you are generating `sendmail.cf` from `sendmail.mc`, add following
    lines after `FEATURE` lines:
    ``` code
    define(`confINPUT_MAIL_FILTERS', `rspamd')
    MAIL_FILTER(`rspamd', `S=inet:11332@localhost')
    ```
    Above is equivalent to below:
    ``` code
    INPUT_MAIL_FILTER(`rspamd', `S=inet:11332@localhost')
    ```

    With sendmail, the authserv-id is the value of `j` macro in `sendmail.cf`
    (in `Dj` line).
    If you are generating `sendmail.cf` from `sendmail.mc`, it is defined
    by `confDOMAIN_NAME` configuration parameter.

  * OpenSMTPD

    It is reported that
    [filter-rspamd](https://github.com/poolpOrg/filter-rspamd)
    (`opensmtpd-filter-rspamd` package on OpenBSD) may be used to integrate
    Rspamd with OpenSMTPD.  For more details see related documents.

    The authserv-id is the value of `hostname` option in `smtpd.conf`.

----

After you finished setting up MTA, [test](dkim-arc-setup-mta.md#tests) it.

