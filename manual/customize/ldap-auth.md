Authentication with email address, uid or alternate email address
-----------------------------------------------------------------

Sympa stores the data relative to the subscribers in a DataBase. Among these data: password, email address exploited during the web authentication. The module of LDAP authentication allows to use Sympa in an intranet without duplicating user passwords.

This way users can indifferently authenticate with their `ldap_uid`, their `alternate_email` or their canonic email stored in the LDAP directory.

Sympa gets the canonic email in the LDAP directory with the `ldap_uid` or the `alternate_email`. Sympa will first attempt an anonymous bind to the directory to get the user's DN, then Sympa will bind with the DN and the user's `ldap_password` in order to perform an efficient authentication. This last bind will work only if the right `ldap_password` is provided. Indeed the value returned by the bind(DN,ldap\_password) is tested.

Example: a person is described by

``` code
dn: cn=Fabrice Rafart, ou=Siege, o=MyCompany, c=FR
objectClass: person
cn: Fabrice Rafart
title: Network Responsible
o: Siege
or: Data processing
telephoneNumber: 01-00-00-00-00
facsimileTelephoneNumber: 01-00-00-00-00
l: Paris
country: France
uid: frafart
mail: Fabrice.Rafart@MyCompany.fr
alternate_email: frafart@MyCompany.fr
alternate: rafart@MyCompany.fr
```

So Fabrice Rafart can be authenticated with: frafart, Fabrice.Rafart@MyCompany.fr, frafart@MyCompany.fr, Rafart@MyCompany.fr. After this operation, the address in the FROM field will be the Canonic email, in this case Fabrice.Rafart@MyCompany.fr. That means that Sympa will get this email and use it during all the session until you clearly ask Sympa to change your email address via the two pages: which and pref.

Example:

``` code
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
