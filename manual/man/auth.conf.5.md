---
title: 'auth.conf(5)'
---

# NAME

auth.conf -
Configuration of authentication mechanisms for web interface of Sympa

# DESCRIPTION

The `auth.conf` configuration file defines authentication mechanisms for web
interface of Sympa.

## `auth.conf` structure

Each paragraph starts with one of the names `user_table`, `ldap`,
`generic_sso` or `cas`.

The `auth.conf` file contains directives in the following format:

    name
    keyword value
    keyword value
    ...
    
    name
    keyword value
    keyword value
    ...

Comments start with the `#` character at the beginning of a line.

Empty lines are also considered as comments and are ignored at the beginning. 
After the first paragraph, they are considered as paragraph separators. There 
should only be one directive per line, but their order in the paragraph is of 
no importance.

Example: enabling both CAS and native authentication:

```
cas
    base_url                        https://yourhost.yourdomain:8443
    auth_service_friendly_name      Your authentication Server
    auth_service_name               your_authn_server
	ldap_host			ldap.yourdomain:389
    ldap_get_email_by_uid_filter    (uid=[uid])
	ldap_timeout			7
	ldap_suffix			dc=yourdomain,dc=fr
	ldap_scope			sub
	ldap_email_attribute		mail

user_table
        regexp                 .*
```
Adding this to auth.conf will transform the login banner into a drop-down menu in which people will get to either fill a username+passwrod (native) or click a button the will redirect them to the CAS server.

Succeeding subsections describe available parameters in each paragraph.

## `user_table` paragraph

This paragraph is related to Sympa internal authentication by email and
password.  Information of users are stored in `user_table` database table.
This is the simplest one.

- `regexp` _regexp_
- `negative_regexp`

Perl regular expressions applied on an email address provided, to select or
block this authentication mechanism for a subset of email addresses.

Example: enabling native authentication for everyone:

```
user_table
        regexp                 .*
```

## `ldap` paragraph

This paragraph allows to login to Sympa using data taken from an LDAP 
directory. Login is done in two steps:

- User provide a user ID or an email address, with a password. These are used 
to retrieve their distinguished name (DN) in the LDAP directory.
- The email attribute is extracted from the directory entry corresponding to 
the found DN.

Here is how to configure the LDAP authentication:

- `regexp`
- `negative_regexp`

    Same as in the `user_table` paragraph: If an email address is provided (this 
    does _not_ apply to the user ID), then the regular expression will be applied 
    to find out if the LDAP directory can be used to authenticate a subset of 
    users.

- `host`

    This keyword is **mandatory**. It is the domain name used in order to bind to 
    the directory and then to extract information. You must mention the port 
    number after the server name. Server replication is supported by listing 
    several servers separated by commas (`,`).

    Example:

        host ldap.univ-rennes1.fr:389

        host ldap0.university.com:389,ldap1.university.com:389,ldap2.university.com:389

- `timeout`

    It corresponds to the time limit in the search operation. A `timelimit` that 
    restricts the maximum time (in seconds) allowed for a search. A value of `0` 
    (the default) means that no time limit will be requested.

- `suffix`

    The root of the DIT (directory information tree). The DN that is the base 
    object entry relative to which the search is to be performed.

    Example:

        dc=university,dc=fr

- `bind_dn`

    If anonymous bind is not allowed on the LDAP server, a DN and password can be 
    used.

- `bind_password`

    This password is used, combined with the `bind_dn` above.

- `get_dn_by_uid_filter`

    Defines the search filter corresponding to the `ldap_uid`. (RFC 2254 
    compliant). If you want to apply the filter on the user, use the variable 
    `[sender]`. It will work with every type of authentication (user ID, 
    `alternate_email`, ...).

    Example:

        (Login = [sender])

        (|(ID = [sender])(UID = [sender]))

- `get_dn_by_email_filter`

    Defines the search filter corresponding to the email addresses (canonic and 
    alternative --- this is RFC 2254 compliant). If you want to apply the filter 
    on the user, use the variable `[sender]`. It will work with every type of 
    authentication (user ID, `alternate_email`..).

    Example: a person is described by

        dn: cn=Fabrice Rafart, ou=Siege, o=MaSociete, c=FR
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
        mail: Fabrice.Rafart@MaSociete.fr
        alternate_email: frafart@MaSociete.fr
        alternate: rafart@MaSociete.fr

    The filters can be:

        (mail = [sender])

        (| (mail = [sender])(alternate_email = [sender]) )

        (| (mail = [sender])(alternate_email = [sender])(alternate  = [sender]) )

- `email_attribute`

    The name of the attribute for the canonic email in your directory: for 
    instance `mail`, `canonic_email`, `canonic_address`, ... In the previous 
    example, the canonic email is `mail`.

- `alternative_email_attribute`

    The name of the attribute for the alternate email in your directory: for 
    instance `alternate_email`, `mailalternateaddress`, ... You make a list of 
    these attributes separated by commas (`,`).

    With this list, Sympa creates a cookie which contains various information: 
    Whether the user is authenticated via LDAP or not, his alternate email. 
    Storing the alternate email is interesting when you want to canonify your 
    preferences and subscriptions, that is to say you want to use a unique 
    address in `user_table` and `subscriber_table`, which is the canonic email.

- `scope`

    Default value: `sub`

    By default, the search is performed on the whole tree below the specified 
    base object. This may be changed by specifying a scope:

    - `base`

        Search only the base object,

    - `one`

        Search the entries immediately below the base object,

    - `sub`

        Search the whole tree below the base object. This is the default.

- `authentication_info_url`

    Defines the URL of a document describing LDAP password management. When 
    hitting Sympa's "Send me a password" button, LDAP users will be redirected to 
    this URL.

### TLS parameters

Following parameters are used to provide LDAPS (LDAP over TLS/SSL):

- `use_ssl` (OBSOLETE)

    If set to `1`, connection to the LDAP server will use LDAPS (LDAP over 
    TLS/SSL).

    Obsoleted as of Sympa 6.2.15. Use `use_tls` instead.

- `use_tls`

    Default value: `none`

    - `ldaps`

        Use LDAPS (LDAP over TLS/SSL),

    - `starttls`

        Use StartTLS,

    - `none`

        TLS (SSL) is disabled.

- `ssl_version`

    Default value: `tlsv1`

    This defines the version of the TLS/SSL protocol to use. Possible values are 
    `sslv2`, `sslv3`, `tlsv1`, `tlsv1_1` and `tlsv1_2`.

- `ssl_ciphers`

    Specify which subset of cipher suites are permissible for this connection, 
    using the standard OpenSSL string format. The default value of Net::LDAPS for 
    ciphers is `ALL`, which permits all ciphers, even those that do not encrypt!

- `ssl_cert`

    Path to client certificate.

    Introduced on Sympa 6.2.

- `ssl_key`

    Path to the secret key of client certificate.

    Introduced on Sympa 6.2.

- `ca_verify`

    `none`, `optional` or `required`. If set to `none`, will never verify 
    server certificate. Latter two need appropriate `ca_path` and/or `ca_file` 
    settings.

    Introduced on Sympa 6.2.

- `ca_path`

    Path to directory store of CA certificates.

    Introduced on Sympa 6.2.

- `ca_file`

    Path to file store of CA certificates.

    Introduced on Sympa 6.2.

Example ldap paragraph:

```
ldap
        host                            ldap1.yourdomain:392,ldap2.yourdomain:392
        timeout                         20
        suffix                          dc=yourOrg,dc=fr
        get_dn_by_uid_filter            (uid=[sender])
        get_dn_by_email_filter          (mail=[sender])
        email_attribute                 mail
        scope                           sub
```

## `generic_sso` paragraph

- `regexp`
- `negative_regexp`

    See [`user_table` paragraph](#user_table-paragraph).

- `service_name`

    This is the SSO service name that will be offered to the user in the login 
    banner menu.

- `service_id`

    This service ID is used as a parameter by Sympa to refer to the SSO service 
    (instead of the service name).

    A corresponding URL on the local web server should be protected by the SSO 
    system; this URL would look like 
    `http://yourhost.yourdomain/sympa/sso_login/inqueue` if the `service_id` is 
    "`inqueue`".

- `http_header_list`

    Sympa gets user attributes from environment variables coming from the web 
    server. These variables are then cached in the `user_table` database table 
    for later use in authorization scenarios (in structure). You can define a 
    comma-separated list of header field names.

- `http_header_prefix`

    Only environment variables starting with the defined prefix will be kept. 
    Another option is to list HTTP header fields explicitly using
    `http_header_list` parameter.

- `email_http_header`

    This parameter defines the environment variable that will contain the 
    authenticated user's email address.

- `http_header_value_separator`

    Default: `;`

    User attributes may be multi-valued (including the user email address. This 
    parameter defines the values separator character(s).

- `logout_url`

    This optional parameter allows to specify the SSO logout URL. If defined, 
    Sympa will redirect the user to this URL after the Sympa logout has been 
    performed.

Example:

```
generic_sso
        service_name       InQueue Federation
        service_id         inqueue
        http_header_prefix HTTP_SHIB
        email_http_header  HTTP_SHIB_EMAIL_ADDRESS
```

### netID mapping parameters

The following parameters define how Sympa can check the user email address, 
either provided by the SSO or by the user themselves:

- `internal_email_by_netid`

    If set to `1`, this parameter makes Sympa use its `netidmap` table to 
    associate net IDs to user email addresses.

- `netid_http_header`

    This parameter defines the environment variable that will contain the user's 
    identifier. This net ID will then be associated with an email address provided 
    by the user.

- `force_email_verify`

    If set to `1`, this parameter makes Sympa check the user's email address. If 
    the email address was not provided by the authentication module, then the 
    user is requested to provide a valid email address.

### LDAP parameters for generic SSO

The following parameters define how Sympa can retrieve the user email 
address; **these are useful only in case the `email_http_header` entry was 
not defined**:

- `ldap_host`

    The LDAP host Sympa will connect to fetch user email. The `ldap_host` 
    include the port number and it may be a comma separated list of redundant
    hosts.

- `ldap_bind_dn`

    The DN used to bind to this server. Anonymous bind is used if this parameter 
    is not defined.

- `ldap_bind_password`

    The password used unless anonymous bind is used.

- `ldap_suffix`

    The LDAP suffix used when searching user email.

- `ldap_scope`

    The scope used when searching user email. Possible values are `sub`, `base` 
    and `one`.

- `ldap_get_email_by_uid_filter`

    The filter used to perform the email search. It can refer to any environment 
    variables inherited from the SSO module, as shown below.

    Example:

        ldap_get_email_by_uid_filter (mail=[SSL_CLIENT_S_DN_Email])

- `ldap_email_attribute`

    The attribute name to be used as user canonical email. In the current version 
    of Sympa, only the first value returned by the LDAP server is used.

- `ldap_timeout`

    The time out for the search.

Example:

```
cas
    base_url                        https://yourhost.yourdomain:8443
    auth_service_friendly_name      Your authentication Server
    auth_service_name               your_authn_server
    ldap_host			ldap.yourdomain:389
    ldap_get_email_by_uid_filter    (uid=[uid])
	ldap_timeout			7
	ldap_suffix			dc=yourdomain,dc=fr
	ldap_scope			sub
	ldap_email_attribute		mail
```

### TLS parameters

To support LDAPS (LDAP over SSL/TLS), corresponding parameters in `ldap` 
paragraph may also be used for `generic_sso`.

## `cas` paragraph

Note that Sympa will act as a CAS client to validate CAS tickets. During this 
exchange, Sympa will check the CAS server X.509 certificate. Therefore you 
should ensure that the certificate authority of the CAS server is known by
Sympa ; this should be configured through the [cafile](./sympa.conf.5.md#cafile) 
or [capath](./sympa.conf.5.md#capath) `sympa.conf` configuration parameters.

- `regexp`
- `negative_regexp`

    See [`user_table` paragraph](#user_table-paragraph).

- `auth_service_name`

    The authentication service name. Note that it is used as an identifier in the 
    code; it should therefore be made of alphanumeric characters only, with no 
    space.

- `auth_service_friendly_name`

    If defined, this string is proposed on the web login banner.

- `host` (OBSOLETE)

    This parameter has been replaced by `base_url` parameter.

- `base_url`

    The base URL of the CAS server.

- `non_blocking_redirection`

    `on` or `off`. Default value: `on`

    This parameter only concerns the first access to Sympa services by a user, it 
    activates or not the non blocking redirection to the related CAS server to 
    check automatically if the user as been previously authenticated with this 
    CAS server. The redirection to CAS is used with the CGI parameter 
    `gateway=1` that specifies to CAS server to always redirect the user to the 
    original URL, but just check if the user is logged. If active, the SSO 
    service is effective and transparent, but in case the CAS server is out of 
    order, the access to Sympa services is impossible.

- `login_uri` (OBSOLETE)

    This parameter has been replaced by the `login_path` parameter.

- `login_path` (OPTIONAL)

    The login service path.

- `check_uri` (OBSOLETE)

    This parameter has been replaced by the `service_validate_path` parameter.

- `service_validate_path` (OPTIONAL)

    The ticket validation service path.

- `logout_uri` (OBSOLETE)

    This parameter has been replaced by the `logout_path` parameter.

- `logout_path` (OPTIONAL)

    The logout service path.

- `proxy_path` (OPTIONAL)

    The proxy service path, only used by the Sympa SOAP server.

- `proxy_validate_path` (OPTIONAL)

    The proxy validate service path, only used by the Sympa SOAP server.

### LDAP parameters for CAS

- `ldap_host`

    The LDAP host Sympa will connect to fetch user email when user uid is return 
    by CAS service. The `ldap_host` includes the port number and it may be a 
    comma separated list of redundant hosts.

- `ldap_bind_dn`

    The DN used to bind to this server. Anonymous bind is used if this parameter 
    is not defined.

- `ldap_bind_password`

    The password used unless anonymous bind is used.

- `ldap_suffix`

    The LDAP suffix used when searching user email.

- `ldap_scope`

    The scope used when searching user email. Possible values are `sub`, `base` 
    and `one`.

- `ldap_get_email_by_uid_filter`

    The filter used to perform the email search.

- `ldap_email_attribute`

    The attribute name to be used as user canonical email. In the current version 
    of Sympa, only the first value returned by the LDAP server is used.

- `ldap_timeout`

    The time out for the search.

### TLS parameters

To support LDAPS (LDAP over SSL/TLS), corresponding parameters in ldap 
paragraph may also be used for cas.

# FILES

- `$DEFAULTDIR/auth.conf`

    Distribution default.  This file should not be edited.

- `$SYSCONFDIR/auth.conf`
- `$SYSCONFDIR/<robot name>/auth.conf`

    Configuration files for site-wide default and each robot.

# SEE ALSO

[wwsympa(8)](./wwsympa.8.md),
[sympa\_soap\_server(8)](./sympa_soap_server.8.md).

[Sympa::Auth](./Sympa-Auth.3.md).

# HISTORY

Descriptions of parameters were originally taken from the chapter
"Authentication" in
_Sympa, Mailing List Management Software - Reference manual_, written by
Serge Aumont, Soji Ikeda, Olivier Salaün and David Verdin.
