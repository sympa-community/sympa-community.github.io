---
title: 'DKIM and ARC: Setup MTA: Using Authentication Milter'
up: setup-mta.md
prev: setup-keys.md
next: setup-mta.md#tests
---

DKIM and ARC: Setup MTA: Using Authentication Milter
====================================================

Requirements
------------

  * MTA: Postfix or Sendmail

  * Authentication Milter, originally distributed as
    [Mail-Milter-Authentication](https://metacpan.org/dist/Mail-Milter-Authentication)
    CPAN module

  * You have to choose **authserv-id** to determine the results of
    domain validation.
    In this document `mx.example.org` is used for example.

Installation
------------

If your operating system provies a package for Authentication Milter,
installing it is recommended.

Otherwise, you may install CPAN module
(In this case, many dependent modules will also be installed,
so you may want to consider using `perlbrew` or similar).

If you use `cpanm`, you can install as follows
(replace `$PREFIX` with the prefix of Perl you are using):
``` bash
# cpanm --notest --install-args "--install_path sbin=$PREFIX/sbin" Mail::SPF
# cpanm --notest Mail::Milter::Authentication
```
Note that some external libraries such as OpenSSL/LibreSSL are required
to build all dependencies.

Authentication Milter also needs some directories.
```bash
# mkdir /var/cache/authentication_milter
# mkdir /var/lib/authentication_milter
# mkdir /var/spool/authentication_milter
```

Configuration
-------------

### Setting Authentication Milter

You have to create `authentication_milter.json` in `/etc` directory
(or appropriate location).
The following are the default settings with minimal modifications.
  - `runas` and `rungroup` are set to unprivileged user and its group.
  - Replace `mx.example.org` with the authserv-id you chose.

``` code
{
    "error_log"   : "/var/log/authentication_milter.err",
    "connection"  : "inet:12345@localhost",
    "umask"       : "0007",
    "runas"       : "nobody",
    "rungroup"    : "nobody",
    "authserv_id" : "mx.example.org",

    "connect_timeout" : 30,
    "command_timeout" : 30,
    "content_timeout" : 300,
    "dns_timeout"     : 10,
    "dns_retry"       : 2,

    "handlers" : {

        "SPF" : {
            "hide_none" : 0
        },

        "DKIM" : {
            "hide_none" : 0,
        },

        "DMARC" : {
            "hide_none" : 0,
            "detect_list_id" : "1"
        },

        "PTR" : {},

        "SenderID" : {
            "hide_none" : 1
        },

        "IPRev" : {},

        "Auth" : {},

        "LocalIP" : {},

        "TrustedIP" : {
            "trusted_ip_list" : []
        },

        "!AddID" : {},

        "ReturnOK" : {},

        "Sanitize" : {}
    }
}
```

If you installed Authentication Milter with CPAN package,
you may also create startup script as necessity.
A sample
[init script](https://github.com/fastmail/authentication_milter/blob/master/share/authentication_milter.init)
is included in the source tarball.

> **Note**
>   * You may also perform unit test of Authentication Milter using
>     `authentication_milter_client`.

### Setting MTA

  * Postfix

    Add following settings to main.cf:
    ``` code
    smtpd_milters = (existing settings) inet:localhost:12345
    milter_default_action = accept
    ```

  * Sendmail

    Edit `sendmail.cf` to add following settings:
    ``` code
    O InputMailFilters=authmilter
    Xauthmilter, S=inet:12345@localhost
    ```
    Or, if you are generating `sendmail.cf` from `sendmail.mc`, add following
    lines after `FEATURE` lines:
    ``` code
    define(`confINPUT_MAIL_FILTERS', `authmilter')
    MAIL_FILTER(`authmilter', `S=inet:12345@localhost')
    ```
    Above is equivalent to below:
    ``` code
    INPUT_MAIL_FILTER(`authmilter', `S=inet:12345@localhost')
    ```

----

After you finished setting up MTA, [test](setup-mta.md#tests) it.

