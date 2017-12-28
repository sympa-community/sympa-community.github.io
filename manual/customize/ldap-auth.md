---
title: 'LDAP authentication'
up: ../customize.md#web-interface-optional-features
---

LDAP authentication
===================

See also "[Authentication on web interface](authentication-web.md)".

The LDAP
[authentication mechanism](authenticate-web.md#authentication-mechanisms)
allows to use Sympa web interface (WWSympa) in an intranet without duplicating
user passwords.

This way users can indifferently authenticate with their "LDAP user ID", their
"alternate email" or their canonic email stored in the LDAP directory.

WWSympa gets the canonic email in the LDAP directory with the "LDAP user ID"
or the "alternate email". WWSympa will first attempt an anonymous bind to the
directory to get the user's distinguished name (DN), then will bind with the
DN and the user's "LDAP password" in order to perform an efficient
authentication. This last bind will work only if the right "LDAP password" is
provided. Indeed the value returned by the LDAP bind operation is tested.

Example: a person is described by

``` code
dn: cn=Fabrice Rafart, ou=Siege, o=MyCompany, c=FR
objectClass: person
cn: Fabrice Rafart
title: Network Responsible
o: Siege
ou: Data processing
telephoneNumber: 01-00-00-00-00
facsimileTelephoneNumber: 01-00-00-00-00
l: Paris
country: France
uid: frafart
mail: Fabrice.Rafart@MyCompany.fr
alternate_email: frafart@MyCompany.fr
alternate: rafart@MyCompany.fr
```

So Fabrice Rafart can be authenticated with `frafart`,
`Fabrice.Rafart@MyCompany.fr`, `frafart@MyCompany.fr` or
`Rafart@MyCompany.fr`. After this operation, the address in the `mail`
attribute will be the canonic email, in this case
`Fabrice.Rafart@MyCompany.fr`. That means that WWSympa will get this email and
use it during all the session until you clearly ask WWSympa to change your
email address using the user preferences page on web interface.

Requirements
------------

  - [Net-LDAP](https://metacpan.org/release/Net-LDAP) Perl module.

  - [IO-Socket-SSL](https://metacpan.org/release/IO-Socket-SSL) Perl module,
    if you need TLS to connect to LDAP server, either with LDAP over TLS
    ("ldaps") or Start_TLS extension.

Sympa configuration
-------------------

You can add LDAP authentication mechanism with
[`ldap`](../man/auth.conf.5.md#ldap-paragraph) paragraph in
[`auth.conf`](../man/auth.conf.5.md) configuration file.  Below is an example
of the paragraph:
```code
ldap
    regexp                      univ-rennes1\.fr
    host                        ldap.univ-rennes1.fr:389
    timeout                     30
    suffix                      dc=univ-rennes1,dc=fr
    get_dn_by_uid_filter        (uid=[sender])
    get_dn_by_email_filter      (|(mail=[sender])(mailalternateaddress=[sender]))
    email_attribute             mail
    alternative_email_attribute mailalternateaddress,ur1mail
    scope                       sub
    use_tls                     ldaps
    ssl_version                 tlsv1
    ssl_ciphers                 MEDIUM:HIGH

ldap
    host                        ldap.univ-nancy2.fr:392,ldap1.univ-nancy2.fr:392,ldap2.univ-nancy2.fr:392
    timeout                     20
    bind_dn                     cn=sympa,ou=people,dc=cru,dc=fr
    bind_password               sympaPASSWD
    suffix                      dc=univ-nancy2,dc=fr
    get_dn_by_uid_filter        (uid=[sender])
    get_dn_by_email_filter      (|(mail=[sender])(n2atraliasmail=[sender]))
    alternative_email_attribute n2atrmaildrop
    email_attribute             mail
    scope                       sub
    authentication_info_url     http://sso.univ-nancy2.fr/
```

