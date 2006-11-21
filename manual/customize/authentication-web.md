Authentication
==============

*Sympa* needs to authenticate users (subscribers, owners, moderators, listmaster) on both its mail and web interface to then apply appropriate privileges (authorization process) to subsequent requested actions. *Sympa* is able to cope with multiple authentication means on the client side and when using user+password it can validate these credentials against LDAP authentication backends.

When contacted on the mail interface *Sympa* has 3 authentication levels. Lower level is to trust the `From:` SMTP header field. A higher level of authentication will require that the user confirms his/her message. The strongest supported authentication method is S/MIME (note that *Sympa* also deals with S/MIME encrypted messages).

On the *Sympa* web interface (*WWSympa*) the user can authenticate in 4 different ways (if appropriate setup has been done on *Sympa* serveur). Default authentication mean is via the user's email address and a password managed by *Sympa* itself. If an LDAP authentication backend (or multiple) has been defined, then the user can authentication with his/her LDAP uid and password. *Sympa* is also able to delegate the authentication job to a web Single SignOn system ; currently [CAS](http://www.yale.edu/tp/auth/) (the Yale University system) or a generic SSO setup, adapted to SSO products providing an Apache module. When contacted via HTTPS, *Sympa* can make use of X509 client certificates to authenticate users.

The authorization process in *Sympa* (authorization scenarios) refers to authentication methods. The same authorization scenarios are used for both mail and web accesss ; therefore some authentication methods are considered as equivalent : mail confirmation (on the mail interface) is equivalent to password authentication (on the web interface) ; S/MIME authentication is equivalent to HTTPS with client certificate authentication. Each rule in authorization scenarios requires an authentication method (`smtp`,`md5` or `smime`) ; if the required authentication method was not used, a higher authentication mode can be requested.

S/MIME and HTTPS authentication
-------------------------------

Chapter [27.2](node28.html#smime-sig) (page [![\[\*\]](crossref.png)](node28.html#smime-sig)) deals with *Sympa* and S/MIME signature. *Sympa* uses `OpenSSL` library to work on S/MIME messages, you need to configure some related *Sympa* parameters : [27.4.3](node28.html#smimeconf) (page [![\[\*\]](crossref.png)](node28.html#smimeconf)).

*Sympa* HTTPS authentication is based on Apache+mod\_SSL that provide the required authentication information via CGI environment variables. You will need to edit Apache configuration to allow HTTPS access and require X509 client certificate. Here is a sample Apache configuration
```
SSLEngine on
SSLVerifyClient optional
SSLVerifyDepth  10
...
<Location /sympa>
   SSLOptions +StdEnvVars
   SetHandler fastcgi-script
</Location>
```
If you are using the SubjAltName, then you additionaly need to export the certificate data because of a mod\_ssl bug. You will also need to install the textindex Crypt-OpenSSL-X509 CPAN module. Add this option to the Apache configuration file :
```
   SSLOptions +ExportCertData
```
 
Authentication with email address, uid or alternate email address
-----------------------------------------------------------------

*Sympa* stores the data relative to the subscribers in a DataBase. Among these data: password, email exploited during the Web authentication. The module of LDAP authentication allows to use *Sympa* in an intranet without duplicating user passwords.

This way users can indifferently authenticate with their ldap\_uid, their alternate\_email or their canonic email stored in the LDAP directory.

*Sympa* gets the canonic email in the LDAP directory with the ldap\_uid or the alternate\_email. *Sympa* will first attempt an anonymous bind to the directory to get the user's DN, then *Sympa* will bind with the DN and the user's ldap\_password in order to perform an efficient authentication. This last bind will work only if the good ldap\_password is provided. Indeed the value returned by the bind(DN,ldap\_password) is tested.

Example: a person is described by
```
dn: cn=Fabrice Rafart, ou=Siege, o=MaSociete, c=FR
objectclass: person
cn: Fabrice Rafart
title: Network Responsible
o: Siege
or: Data processing
telephoneNumber: 01-00-00-00-00
facsimileTelephoneNumber: 01-00-00-00-00
l: Paris
country: France
uid: frafart
mail: Fabrice.Rafart@MaSociete.fr
alternate_email: frafart@MaSociete.fr
alternate: rafart@MaSociete.fr
```
So Fabrice Rafart can be authenticated with: frafart, Fabrice.Rafart@MaSociete.fr, frafart@MaSociete.fr,Rafart@MaSociete.fr. After this operation, the address in the field FROM will be the Canonic email, in this case Fabrice.Rafart@MaSociete.fr. That means that *Sympa* will get this email and use it during all the session until you clearly ask *Sympa* to change your email address via the two pages : which and pref.

 
Generic SSO authentication
--------------------------

The authentication method has first been introduced to allow interraction with [Shibboleth](http://shibboleth.internet2.edu/), Internet2's inter-institutional authentication system. But it should be usable with any SSO system that provides an Apache authentication module being able to protect a specified URL on the site (not the whole site). Here is a sample httpd.conf that shib-protects the associated Sympa URL :
```
...
<Location /sympa/sso_login/inqueue>
    AuthType shibboleth
    require affiliation ~ ^member@.+
</Location>
...
```
*Sympa* will get user attributes via environment variables. In the most simple case the SSO will provide the user email address. If not, Sympa can be configured to verify an email address provided by the user hiself or to look for the user email address in a LDAP directory (the search filter will make use of user information inherited from the SSO Apache module).

To plug a new SSO server in your Sympa server you should add a **generic\_sso** paragraph (describing the SSO service) in your `auth.conf` configuration file (See [13.5.3](#generic-sso-format), page [![\[\*\]](crossref.png)](node14.html#generic-sso-format)). Once this paragraph has been added, the SSO service name will be automatically added to the web login menu.

Apart from the user email address, the SSO can provide other user attributes that *Sympa* will store in the user\_table DB table (for persistancy) and make them available in the \[user\_attributes\] structure that you can use within authorization scenarios (see [14.1](node15.html#rules), page [![\[\*\]](crossref.png)](node15.html#rules)) or in web templates via the \[% user.attributes %\] structure.

 
CAS-based authentication
------------------------

CAS is Yale university SSO software. Sympa can use CAS authentication service.

The listmaster should define at least one or more CAS servers (**cas** paragraph) in `auth.conf`. If **non\_blocking\_redirection** parameter was set for a CAS server then Sympa will try a transparent login on this server when the user accesses the web interface. If one CAS server redirect the user to Sympa with a valid ticket Sympa receives a user ID from the CAS server. It then connects to the related LDAP directory to get the user email address. If no CAS server returns a valid user ID, Sympa will let the user either select a CAS server to login or perform a Sympa login.

 
auth.conf
---------

The `/usr/local/sympa-os/etc/auth.conf` configuration file contains numerous parameters which are read on start-up of *Sympa*. If you change this file, do not forget that you will need to restart wwsympa.fcgi afterwards.

The `/usr/local/sympa-os/etc/auth.conf` is organised in paragraphs. Each paragraph describes an authentication service with all required parameters to perform an authentication using this service. Current version of *Sympa* can perform authentication through LDAP directories, using an external Single Sign-On Service (like CAS or Shibboleth), or using internal user\_table.

The login page contains 2 forms : the login form and the SSO. When users hit the login form, each ldap or user\_table authentication paragraph is applied unless email adress input from form match the `negative_regexp` or do not match `regexp`. `negative_regexp` and `regexp` can be defined for earch ldap or user\_table authentication service so administrator can block some authentication methode for class of users.

The segond form in login page contain the list of CAS server so user can choose explicitely his CAS service.

Each paragraph start with one of the keyword cas, ldap or user\_table

The `/usr/local/sympa-os/etc/auth.conf` file contains directives in the following format:

> *paragraphs*
> *keyword value*
> 
> *paragraphs*
> *keyword value*

Comments start with the `#` character at the beginning of a line.

Empty lines are also considered as comments and are ignored at the beginning. After the first paragraph they are considered as paragraphs separators. There should only be one directive per line, but their order in the paragraph is of no importance.

Example :
```
#Configuration file auth.conf for the LDAP authentification
#Description of parameters for each directory

cas
    base_url            https://sso-cas.cru.fr
    non_blocking_redirection        on
    auth_service_name       cas-cru
    ldap_host           ldap.cru.fr:389
    ldap_get_email_by_uid_filter    (uid=[uid])
    ldap_timeout            7
    ldap_suffix         dc=cru,dc=fr
    ldap_scope          sub
    ldap_email_attribute        mail

## The URL corresponding to the service_id should be protected by the SSO (Shibboleth in the exampl)
## The URL would look like http://yourhost.yourdomain/sympa/sso_login/inqueue in the following example
generic_sso
    service_name       InQueue Federation
    service_id         inqueue
    http_header_prefix HTTP_SHIB
    email_http_header  HTTP_SHIB_EMAIL_ADDRESS

## The email address is not provided by the user home institution
generic_sso
    service_name               Shibboleth Federation
    service_id                 myfederation
    http_header_prefix         HTTP_SHIB
    netid_http_header          HTTP_SHIB_EMAIL_ADDRESS
    internal_email_by_netid    1
    force_email_verify         1

ldap
    regexp              univ-rennes1\.fr
    host                ldap.univ-rennes1.fr:389
    timeout             30
    suffix              dc=univ-rennes1,dc=fr
    get_dn_by_uid_filter        (uid=[sender])
    get_dn_by_email_filter      (|(mail=[sender])(mailalternateaddress=[sender]))
    email_attribute         mail
    alternative_email_attribute mailalternateaddress,ur1mail
    scope               sub
    use_ssl                         1
    ssl_version                     sslv3
    ssl_ciphers                     MEDIUM:HIGH

ldap
    host                ldap.univ-nancy2.fr:392,ldap1.univ-nancy2.fr:392,ldap2.univ-nancy2.fr:392
    timeout             20
    bind_dn                         cn=sympa,ou=people,dc=cru,dc=fr
    bind_password                   sympaPASSWD
    suffix              dc=univ-nancy2,dc=fr
    get_dn_by_uid_filter        (uid=[sender])
    get_dn_by_email_filter          (|(mail=[sender])(n2atraliasmail=[sender]))
    alternative_email_attribute n2atrmaildrop
    email_attribute         mail
    scope               sub
    authentication_info_url         http://sso.univ-nancy2.fr/
    

user_table
    negative_regexp         ((univ-rennes1)|(univ-nancy2))\.fr
```

### user\_table paragraph

The user\_table paragraph is related to sympa internal authentication by email and password. It is the simplest one the only parameters are `regexp` or `negative_regexp` which are perl regular expressions applied on a provided email address to select or block this authentication method for a subset of email addresses.

### ldap paragraph

  - `regexp` and `negative_regexp` Same as in user\_table paragraph : if a provided email address (does not apply to an uid), then the regular expression will be applied to find out if this LDAP directory can be used to authenticate a subset of users.
  - host

    This keyword is **mandatory**. It is the domain name used in order to bind to the directory and then to extract informations. You must mention the port number after the server name. Server replication is supported by listing several servers separated by commas.

    Example :
    ```
    host ldap.univ-rennes1.fr:389
    host ldap0.university.com:389,ldap1.university.com:389,ldap2.university.com:389
    ```

  - timeout

    It corresponds to the timelimit in the Search fonction. A timelimit that restricts the maximum time (in seconds) allowed for a search. A value of 0 (the default), means that no timelimit will be requested.

  - suffix

    The root of the DIT (Directory Information Tree). The DN that is the base object entry relative to which the search is to be performed.

    Example: `dc=university,dc=fr`

  - bind\_dn

    If anonymous bind is not allowed on the LDAP server, a DN and password can be used.

  - bind\_password

    This password is used, combined with the bind\_dn above.

  - get\_dn\_by\_uid\_filter

    Defines the search filter corresponding to the ldap\_uid. (RFC 2254 compliant). If you want to apply the filter on the user, use the variable ' \[sender\] '. It will work with every type of authentication (uid, alternate\_email..).

    Example :
    ```
    (Login = [sender])
    ```
    ```
    (|(ID = [sender])(UID = [sender]))
    ```

  - get\_dn\_by\_email\_filter

    Defines the search filter corresponding to the email addresses (canonic and alternative).(RFC 2254 compliant). If you want to apply the filter on the user, use the variable ' \[sender\] '. It will work with every type of authentication (uid, alternate\_email..).

    Example: a person is described by
    ```
    dn: cn=Fabrice Rafart, ou=Siege, o=MaSociete, c=FR
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
    mail: Fabrice.Rafart@MaSociete.fr
    alternate_email: frafart@MaSociete.fr
    alternate: rafart@MaSociete.fr
    ```
    The filters can be :
    ```
    (mail = [sender])
    ```
    ```
    (| (mail = [sender])(alternate_email = [sender]) )
    ```
    ```
    (| (mail = [sender])(alternate_email = [sender])(alternate  = [sender]) )
    ```

  - email\_attribute

    The name of the attribute for the canonic email in your directory : for instance mail, canonic\_email, canonic\_address ... In the previous example the canonic email is 'mail'.

  - alternative\_email\_attribute

    The name of the attribute for the alternate email in your directory : for instance alternate\_email, mailalternateaddress, ... You make a list of these attributes separated by commas.

    With this list *Sympa* creates a cookie which contains various information : the user is authenticated via Ldap or not, his alternate email. To store the alternate email is interesting when you want to canonify your preferences and subscriptions. That is to say you want to use a unique address in User\_table and Subscriber\_table which is the canonic email.

  - scope

    (Default value: `sub`) By default the search is performed on the whole tree below the specified base object. This may be changed by specifying a scope :

    -   base
        Search only the base object.
    -   one
        Search the entries immediately below the base object.
    -   sub
        Search the whole tree below the base object. This is the default.

  - authentication\_info\_url

    Defines the URL of a document describing LDAP password management. When hitting Sympa's *Send me a password* button, LDAP users will be redirected to this URL.

  - use\_ssl

    If set to `1`, connection to the LDAP server will use SSL (LDAPS).

  - ssl\_version

    This defines the version of the SSL/TLS protocol to use. Defaults of Net::LDAPS to `sslv2/3`, other possible values are `sslv2`, `sslv3`, and `tlsv1`.

  - ssl\_ciphers

    Specify which subset of cipher suites are permissible for this connection, using the standard OpenSSL string format. The default value of Net::LDAPS for ciphers is `ALL`, which permits all ciphers, even those that don't encrypt!

 
### generic\_sso paragraph

  - service\_name
    This is the SSO service name that will be proposed to the user in the login banner menu.
  - service\_id
    This service ID is used as a parameter by sympa to refer to the SSO service (instead of the service name).

    A corresponding URL on the local web server should be protected by the SSO system ; this URL would look like **http://yourhost.yourdomain/sympa/sso\_login/inqueue** if the service\_id is **inqueue**.

  - http\_header\_prefix
    Sympa gets user attributes from environment variables comming from the web server. These variables are then stored in the user\_table DB table for later use in authorization scenarios (in structure). Only environment variables starting with the defined prefix will kept.
  - email\_http\_header
    This parameter defines the environment variable that will contain the authenticated user's email address.

The following parameters define how Sympa can verify the user email address, either provided by the SSO or by the user himself :

  - internal\_email\_by\_netid
    If set to 1 this parameter makes Sympa use its netidmap table to associate NetIDs to user email address.
  - netid\_http\_header
    This parameter defines the environment variable that will contain the user's identifier. This netid will then be associated with an email address either provided by the user.
  - force\_email\_verify
    If set to 1 this parameter makes Sympa verify the user's email address. If the email address was not provided by the authentication module, then the user is requested to provide a valid email address.

The following parameters define how Sympa can retrieve the user email address ; **these are only useful if the email\_http\_header entry was not defined :**

  - ldap\_host
    The LDAP host Sympa will connect to fetch user email. The ldap\_host include the port number and it may be a comma separated list of redondant host.
  - ldap\_bind\_dn
    The DN used to bind to this server. Anonymous bind is used if this parameter is not defined.
  - ldap\_bind\_password
    The password used unless anonymous bind is used.
  - ldap\_suffix
    The LDAP suffix used when seraching user email
  - ldap\_scope
    The scope used when seraching user email, possible values are `sub`, `base`, and `one`.
  - ldap\_get\_email\_by\_uid\_filter
    The filter to perform the email search. It can refer to any environment variables inherited from the SSO module, as shown below. Example :
    ```
    ldap_get_email_by_uid_filter    (mail=[SSL_CLIENT_S_DN_Email])
    ```
  - ldap\_email\_attribute
    The attribut name to be used as user canonical email. In the current version of sympa only the first value returned by the LDAP server is used.
  - ldap\_timeout
    The time out for the search.
  - ldap\_use\_ssl

    If set to `1`, connection to the LDAP server will use SSL (LDAPS).

  - ldap\_ssl\_version

    This defines the version of the SSL/TLS protocol to use. Defaults of Net::LDAPS to `sslv2/3`, other possible values are `sslv2`, `sslv3`, and `tlsv1`.

  - ldap\_ssl\_ciphers

    Specify which subset of cipher suites are permissible for this connection, using the OpenSSL string format. The default value of Net::LDAPS for ciphers is `ALL`, which permits all ciphers, even those that don't encrypt!

### cas paragraph

  - auth\_service\_name
    The friendly user service name as shown by *Sympa* in the login page.
  - host (OBSOLETE)
    This parameter has been replaced by **base\_url** parameter
  - base\_url

    The base URL of the CAS server.

  - non\_blocking\_redirection

    This parameter concern only the first access to Sympa services by a user, it activate or not the non blocking redirection to the related cas server to check automatically if the user as been previously authenticated with this CAS server. Possible values are **on** **off**, default is **on**. The redirection to CAS is use with the cgi parameter **gateway=1** that specify to CAS server to always redirect the user to the origine URL but just check if the user is logged. If active, the SSO service is effective and transparent, but in case the CAS server is out of order the access to Sympa services is impossible.

  - login\_uri (OBSOLETE)
    This parameter has been replaced by **login\_path** parameter.
  - login\_path (OPTIONAL)
    The login service path
  - check\_uri (OBSOLETE)
    This parameter has been replaced by **service\_validate\_path** parameter
  - service\_validate\_path (OPTIONAL)
    The ticket validation service path
  - logout\_uri (OBSOLETE)
    This parameter has been replaced by **logout\_path** parameter
  - logout\_path (OPTIONAL)
    The logout service path
  - proxy\_path (OPTIONAL)
    The proxy service path, used by Sympa SOAP server only.
  - proxy\_validate\_path (OPTIONAL)
    The proxy validate service path, used by Sympa SOAP server only.
  - ldap\_host
    The LDAP host Sympa will connect to fetch user email when user uid is return by CAS service. The ldap\_host include the port number and it may be a comma separated list of redondant host.
  - ldap\_bind\_dn
    The DN used to bind to this server. Anonymous bind is used if this parameter is not defined.
  - ldap\_bind\_password
    The password used unless anonymous bind is used.
  - ldap\_suffix
    The LDAP suffix used when seraching user email
  - ldap\_scope
    The scope used when seraching user email, possible values are `sub`, `base`, and `one`.
  - ldap\_get\_email\_by\_uid\_filter
    The filter to perform the email search.
  - ldap\_email\_attribute
    The attribut name to be use as user canonical email. In the current version of sympa only the first value returned by the LDAP server is used.
  - ldap\_timeout
    The time out for the search.
  - ldap\_use\_ssl

    If set to `1`, connection to the LDAP server will use SSL (LDAPS).

  - ldap\_ssl\_version

    This defines the version of the SSL/TLS protocol to use. Defaults of Net::LDAPS to `sslv2/3`, other possible values are `sslv2`, `sslv3`, and `tlsv1`.

  - ldap\_ssl\_ciphers

    Specify which subset of cipher suites are permissible for this connection, using the OpenSSL string format. The default value of Net::LDAPS for ciphers is `ALL`, which permits all ciphers, even those that don't encrypt!

 
Sharing *WWSympa* authentication with other applications
--------------------------------------------------------

If your are not using a web Single SignOn system you might want to make other web applications collaborate with *Sympa*, and share the same authentication system. *Sympa* uses HTTP cookies to carry users' auth information from page to page. This cookie carries no information concerning privileges. To make your application work with *Sympa*, you have two possibilities :

  - Delegating authentication operations to *WWSympa*
    If you want to avoid spending a lot of time programming a CGI to do Login, Logout and Remindpassword, you can copy *WWSympa*'s login page to your application, and then make use of the cookie information within your application. The cookie format is :
    ```
    sympauser=<user_email>:<checksum>
    ```
    where `<`user\_email`>` is the user's complete e-mail address, and `<`checksum`>` are the 8 last bytes of the a MD5 checksum of the `<`user\_email`>`+*Sympa* `cookie` configuration parameter. Your application needs to know what the `cookie` parameter is, so it can check the HTTP cookie validity ; this is a secret shared between *WWSympa* and your application. *WWSympa*'s *loginrequest* page can be called to return to the referer URL when an action is performed. Here is a sample HTML anchor :
    ```
    <A HREF="/sympa/loginrequest/referer">Login page</A>
    ```
    You can also have your own HTML page submitting data to `wwsympa.fcgi` CGI. If you're doing so, you can set the `referer` variable to another URI. You can also set the `failure_referer` to make WWSympa redirect the client to a different URI if login fails.

  - Using *WWSympa*'s HTTP cookie format within your auth module
    To cooperate with *WWSympa*, you simply need to adopt its HTTP cookie format and share the secret it uses to generate MD5 checksums, i.e. the `cookie` configuration parameter. In this way, *WWSympa* will accept users authenticated through your application without further authentication.

 
Provide a Sympa login form in another application
-------------------------------------------------

You can easily trigger a Sympa login from within another web page. The login form should look like this :
```
<FORM ACTION="http://listes.cru.fr/sympa" method="post">
    <input type="hidden" name="previous_action" value="arc" />
    Access web archives of list
    <select name="previous_list">
    <option value="sympa-users" >sympa-users</option>
    </select><br/>

    <input type="hidden" name="action" value="login" />
    <label for="email">email address :
    <input type="text" name="email" id="email" size="18" value="" /></label><br />
    <label for="passwd" >password :
    <input type="password" name="passwd" id="passwd" size="8" /></label> <br/>
    <input class="MainMenuLinks" type="submit" name="action_login" value="Login and access web archives" />
</FORM>
```
The example above does not only perform the login action but also redirects the user to another sympa page, a list web archives here. The `previous_action` and `previous_list` variable define the action that will be performed after the login is done.

