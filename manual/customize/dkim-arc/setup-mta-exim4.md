---
title: 'DKIM and ARC: Setup MTA: Exim'
up: setup-mta.md
prev: setup-keys.md
next: setup-mta.md#tests
---

DKIM and ARC: Setup MTA: Exim
=============================

Requirements
------------

  * Exim 4.91 or later.

    > **Note**
    >
    >   * Validation of DKIM, SPF, and DMARC must be enabled when Exim is
    >     built (it usually should be).

  * You have to choose **authserv-id** to determine the results of domain
    validation.
    In this document `mx.example.org` is used for example.

Configuration
-------------

### Setting Exim

Note that,
Exim also has DKIM signing capability, but we may not configure them:
Sympa is responsible for that feature.

Thus, basically the configuration of Exim we have to do for Sympa is just
adding following line at the end of the DATA ACL which is usually named
`acl_check_data` (Note: Replace `mx.example.org`).

``` code
  warn    add_header = :at_start: ${authresults {mx.example.org}}
```

If you chose single monolithic configuration,
`acl_check_data` is included in the `acl` section (the part after
"`begin acl`" and before "`begin `_otherThing_").
Otherwise, if you chose splitted configuration, it is included in the file
under `acl` directory, for example
`/etc/exim/conf.d/acl/40_exim4-config_check_data`.

For details about DKIM / SPF / DMARC validation by Exim, see the
[documentation](https://www.exim.org/exim-html-current/doc/html/spec_html/ch-dkim_spf_srs_and_dmarc.html).

----

After you finished setting up MTA, [test](setup-mta.md#tests) it.

