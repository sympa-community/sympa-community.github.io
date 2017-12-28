CAS-based authentication
------------------------

CAS is the Yale University SSO software. Sympa can use the CAS authentication service.

Listmasters should define at least one or more CAS servers (**cas** paragraph) in `auth.conf`. If the `non_blocking_redirection` parameter was set for a CAS server, then Sympa will try a transparent login on this server when the user accesses the web interface. If a CAS server redirects the user to Sympa with a valid ticket, Sympa receives a user ID from the CAS server. Then, it connects to the related LDAP directory to get the user email address. If no CAS server returns a valid user ID, Sympa will let the user either select a CAS server to login or perform a Sympa login.

Example:

``` code
cas
    base_url            https://sso-cas.renater.fr
    non_blocking_redirection        on
    auth_service_name        cas-cru
    ldap_host            ldap.renater.fr:389
    ldap_get_email_by_uid_filter    (uid=[uid])
    ldap_timeout            7
    ldap_suffix            dc=cru,dc=fr
    ldap_scope            sub
    ldap_email_attribute        mail
```
