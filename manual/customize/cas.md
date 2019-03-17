---
title: 'CAS single sign-on'
up: ../customize.md#web-interface-optional-features
---

CAS single sign-on
==================

See also "[Authentication on web interface](authentication-web.md)".

[CAS](https://developers.yale.edu/documentation/Administrative/cas) is the
Yale University SSO software. Sympa's web interface (WWSympa) provides
[authentication mechanism](authentication-web.md#authentication-mechanisms)
to use the CAS authentication service.

If a CAS server redirects the user to WWSympa with a valid ticket, WWSympa
receives a user ID from the CAS server. Then, it connects to the related LDAP
directory to get the user email address. If no CAS server returns a valid
user ID, WWSympa will let the user either select a CAS server to login or
perform another authentication mechanism.

Requirements
------------

  - CAS authentication server is running and accessible from users' local
    network.

  - [AuthCAS](https://metacpan.org/release/AuthCAS) Perl module has to be
    installed.

Sympa configuration
-------------------

Listmasters should define one or more
[`cas`](/gpldoc/man/auth.conf.5.html#cas-paragraph) paragraph in `auth.conf`
configuration file. Below is an example of the paragraph:
``` code
cas
    base_url                        https://sso-cas.renater.fr
    non_blocking_redirection        on
    auth_service_name               cas-cru
    ldap_host                       ldap.renater.fr:389
    ldap_get_email_by_uid_filter    (uid=[uid])
    ldap_timeout                    7
    ldap_suffix                     dc=cru,dc=fr
    ldap_scope                      sub
    ldap_email_attribute            mail
```

Note:

  - `base_url` specifys base URL of CAS server.

  - If the `non_blocking_redirection` parameter was set for a CAS server, then
    WWSympa will try a transparent login on this server when the user accesses
    the web interface.

  - `ldap_*` parameters defines how to map CAS user IDs to email addresses of
    Sympa users.  The variable `[uid]` in `ldap_get_email_by_uid_filter`
    parameter will be replaced with CAS user ID.

