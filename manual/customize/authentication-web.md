Authentication
==============

Sympa needs to authenticate users (subscribers, owners, moderators, listmasters) on both its mail and web interface, and then apply appropriate privileges (authorization process) to subsequent requested actions. Sympa is able to cope with multiple authentication means on the client side, and when using user+password, it can validate these credentials against LDAP authentication backends.

When contacted on the mail interface,Sympa has 4 authentication levels Lower level is to trust the `From:` SMTP header field. A higher level of authentication will require that the user confirms his/her message. The strongest supported authentication method is S/MIME (note that Sympa also deals with S/MIME encrypted messages).

On the Sympa web interface (*WWSympa*) the user can authenticate in 4 different ways (if appropriate setup has been done on the Sympa server). Default authentication is performed through the user's email address and a password managed by Sympa itself. If an LDAP authentication backend (or multiple) has been defined, then the user can authenticate with his/her LDAP uid and password. Sympa is also able to delegate the authentication job to a web Single SignOn system; currently [CAS](http://www.yale.edu/tp/auth/) (the Yale University system) or a generic SSO setup, adapted to SSO products providing an Apache module. When contacted via HTTPS, Sympa can make use of X509 client certificates to authenticate users.

The authorization process in Sympa (authorization scenarios) refers to authentication methods. The same authorization scenarios are used for both mail and web accesss; therefore some authentication methods are considered to be equivalent: mail confirmation (on the mail interface) is equivalent to password authentication (on the web interface); S/MIME authentication is equivalent to HTTPS with client certificate authentication. Each rule in authorization scenarios requires an authentication method (`smtp`, `md5` or `smime`); if the required authentication method was not used, a higher authentication mode can be requested.

S/MIME and HTTPS authentication
-------------------------------

Chapter [Use of S/MIME signature by Sympa itself](/manual/x509#use_of_smime_signatures_by_sympa_itself) deals with Sympa and S/MIME signature. Sympa uses the `OpenSSL` library to work on S/MIME messages, you need to configure some related Sympa parameters: [S/X509 Sympa configuration](/manual/x509#ssympa_configuration).

Sympa HTTPS authentication is based on Apache+mod\_SSL that provide the required authentication information through CGI environment variables. You will need to edit the Apache configuration to allow HTTPS access and require X509 client certificate. Here is a sample Apache configuration:

``` code
SSLEngine on
SSLVerifyClient optional
SSLVerifyDepth  10
...
<Location /sympa>
    SSLOptions +StdEnvVars
    SetHandler fastcgi-script
</Location>
```

If you are using the SubjAltName, then you additionaly need to export the certificate data because of a `mod_ssl` bug. You will also need to install the textindex Crypt-OpenSSL-X509 CPAN module. Add this option to the Apache configuration file:

``` code
SSLOptions +ExportCertData
```

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

Generic SSO authentication
--------------------------

The authentication method has first been introduced to allow interraction with [Shibboleth](http://shibboleth.internet2.edu/), Internet2's inter-institutional authentication system. But it should be usable with any SSO system that provides an Apache authentication module being able to protect a specified URL on the site (not the whole site). Here is a sample `httpd.conf` that shib-protects the associated Sympa URL:

``` code
...
<Location /sympa/sso_login/inqueue>
    AuthType shibboleth
    require mail ~ @
</Location>
...
```

Sympa will get user attributes via environment variables. In the most simple case, the SSO will provide the user email address. If not, Sympa can be configured to check an email address provided by the user, or to look for the user email address in a LDAP directory (the search filter will make use of user information inherited from the SSO Apache module).

To plug a new SSO server in your Sympa server, you should add a `generic_sso` paragraph (describing the SSO service) in your `auth.conf` configuration file (see [generic_sso paragraph](/manual/authentication#generic_sso_paragraph)). Once this paragraph has been added, the SSO service name will be automatically added to the web login menu.

Apart from the user email address, the SSO can provide other user attributes that Sympa will store in the `user_table` DB table (for persistancy), and make available in the \[user\_attributes\] structure that you can use within authorization scenarios (see [Rules specifications](/manual/authorization-scenarios#rules_specifications)) or in web templates via the \[% user.attributes %\] structure.

CAS-based authentication
------------------------

CAS is the Yale University SSO software. Sympa can use the CAS authentication service.

Listmasters should define at least one or more CAS servers (**cas** paragraph) in `auth.conf`. If the `non_blocking_redirection` parameter was set for a CAS server, then Sympa will try a transparent login on this server when the user accesses the web interface. If a CAS server redirects the user to Sympa with a valid ticket, Sympa receives a user ID from the CAS server. Then, it connects to the related LDAP directory to get the user email address. If no CAS server returns a valid user ID, Sympa will let the user either select a CAS server to login or perform a Sympa login.

auth.conf
---------

The `/home/sympa/etc/auth.conf` configuration file contains numerous parameters which are read on start-up of Sympa. If you change this file, do not forget that you will need to restart `wwsympa.fcgi` afterwards.

The `/home/sympa/etc/auth.conf` is organized in paragraphs. Each paragraph describes an authentication service with all parameters required to perform an authentication using this service. Sympa's current version can perform authentication through LDAP directories, using an external Single Sign-On Service (like CAS or Shibboleth), or using the internal `user_table` table.

### ''regexp'' and ''negative\_regexp'' : the auth.conf switches

Suppose your organization use two domains for its email addresses, for example "student.univ.edu" and "univ.edu". The first domain correspond to people stored in a part of the LDAP directory , and the other one to people stored in another part. You may want to define a specific authentication paragraph for each of these groups.

the `regexp` subparameter – and its evil twin `negative_regexp` – are exactly used to perform such a distinction: it allows you to apply different authentication paragraph based on which email address was provided by the user. Let's emphasize this: **the `regexp` and `negative_regexp` are applied to email addresses *only*. It will *not* work on user id.**

Each paragraph in `auth.conf` can contain an occurence of these subparameters. Their value is a regexp (just the regexp part. No delimiters).

Example:

``` code
regexp         student\.univ\.edu
```

What do they do?

They are tested among the domain part of the email provided. The paragraph will be used with this email address if it matches the expression defined by `regexp` or if it does *not* match the expressino defined by `negative_regexp`.

### Login form

The login page contains 2 forms: the login form and the SSO. When users hit the login form, each ldap or `user_table` authentication paragraph is applied unless email adress input from form matches the `negative_regexp` or do not match `regexp`. `negative_regexp` and `regexp` can be defined for each ldap or `user_table` authentication service so that administrators can block some authentication methods for a class of users.

The second form in the login page contains the list of CAS servers so that users can choose explicitely their CAS service.

### ''auth.conf'' structure

Each paragraph starts with one of the keyword `cas`, `ldap` or `user_table`.

The `/home/sympa/etc/auth.conf` file contains directives in the following format:

``` code
paragraphs
keyword value

paragraphs
keyword value
```

Comments start with the `#` character at the beginning of a line.

Empty lines are also considered as comments and are ignored at the beginning. After the first paragraph, they are considered as paragraph separators. There should only be one directive per line, but their order in the paragraph is of no importance.

Example:

``` code
#Configuration file auth.conf for the LDAP authentification
#Description of parameters for each directory

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

## The URL corresponding to the service_id should be protected by the SSO (Shibboleth in the exampl)
## The URL would look like http://yourhost.yourdomain/sympa/sso_login/inqueue in the following example
generic_sso
    service_name       InQueue Federation
    service_id         inqueue
    http_header_list   mail,displayName,eduPersonAffiliation
    email_http_header  mail

## The email address is not provided by the user home institution
generic_sso
    service_name               Shibboleth Federation
    service_id                 myfederation
    http_header_list           mail,displayName,eduPersonAffiliation
    netid_http_header          mail
    internal_email_by_netid    1
    force_email_verify         1

ldap
    regexp                univ-rennes1\.fr
    host                ldap.univ-rennes1.fr:389
    timeout                30
    suffix                dc=univ-rennes1,dc=fr
    get_dn_by_uid_filter        (uid=[sender])
    get_dn_by_email_filter        (|(mail=[sender])(mailalternateaddress=[sender]))
    email_attribute            mail
    alternative_email_attribute    mailalternateaddress,ur1mail
    scope                sub
    use_tls                         ldaps
    ssl_version                     tlsv1
    ssl_ciphers                     MEDIUM:HIGH

ldap
    host                ldap.univ-nancy2.fr:392,ldap1.univ-nancy2.fr:392,ldap2.univ-nancy2.fr:392
    timeout                20
    bind_dn                         cn=sympa,ou=people,dc=cru,dc=fr
    bind_password                   sympaPASSWD
    suffix                dc=univ-nancy2,dc=fr
    get_dn_by_uid_filter        (uid=[sender])
    get_dn_by_email_filter            (|(mail=[sender])(n2atraliasmail=[sender]))
    alternative_email_attribute    n2atrmaildrop
    email_attribute            mail
    scope                sub
    authentication_info_url         http://sso.univ-nancy2.fr/

user_table
    negative_regexp         ((univ-rennes1)|(univ-nancy2))\.fr
```

### user\_table paragraph

The `user_table` paragraph is related to Sympa internal authentication by email and password. It is the simplest one. The only parameters are `regexp` or `negative_regexp` which are Perl regular expressions applied on an email address provided, to select or block this authentication method for a subset of email addresses.

### ldap paragraph

This paragraph allows to login to Sympa using data taken from an LDAP directory. Login is done in two steps:

  - user provide an user id or an email address, with a password. These are used to retrieve their DN in the LDAP directory.

  - the email attribute is extracted from the directory entry corresponding to the found DN.

Here is how to configure the LDAP authentication:

  - `regexp` and `negative_regexp`
    Same as in the `user_table` paragraph: if an email address is provided (this does not apply to an uid), then the regular expression will be applied to find out if the LDAP directory can be used to authenticate a subset of users.

  - `host`
    This keyword is **mandatory**. It is the domain name used in order to bind to the directory and then to extract information. You must mention the port number after the server name. Server replication is supported by listing several servers separated by commas.

    Example:
    ``` code
    host ldap.univ-rennes1.fr:389
    ```
    ``` code
    host ldap0.university.com:389,ldap1.university.com:389,ldap2.university.com:389
    ```

  - `timeout`
    It corresponds to the timelimit in the Search fonction. A timelimit that restricts the maximum time (in seconds) allowed for a search. A value of 0 (the default) means that no timelimit will be requested.

  - `suffix`
    The root of the DIT (Directory Information Tree). The DN that is the base object entry relative to which the search is to be performed.

    Example: `dc=university,dc=fr`

  - `bind_dn`
    If anonymous bind is not allowed on the LDAP server, a DN and password can be used.

  - `bind_password`
    This password is used, combined with the `bind_dn` above.

  - `get_dn_by_uid_filter`
    Defines the search filter corresponding to the `ldap_uid`. (RFC 2254 compliant). If you want to apply the filter on the user, use the variable ' \[sender\] '. It will work with every type of authentication (uid, `alternate_email`, ...).

    Example:
    ``` code
    (Login = [sender])
    ```
    ``` code
    (|(ID = [sender])(UID = [sender]))
    ```

  - `get_dn_by_email_filter`
    Defines the search filter corresponding to the email addresses (canonic and alternative - this is RFC 2254 compliant). If you want to apply the filter on the user, use the variable ' \[sender\] '. It will work with every type of authentication (`uid`, `alternate_email`..).

    Example: a person is described by
    ``` code
    dn: cn=Fabrice Rafart, ou=Siege, o=MaSociete, c=FR
    objectClass: person
    cn: Fabrice Rafart
    title: Network Responsible
    o: Siege
    or: Data processing
    telephoneNumber: 01-00-00-00-00
    facsimileTelephoneNumber: 01-00-00-00-00
    l:Paris
    country: France
    uid: frafart
    mail: Fabrice.Rafart@MaSociete.fr
    alternate_email: frafart@MaSociete.fr
    alternate: rafart@MaSociete.fr
    ```
    The filters can be:
    ``` code
    (mail = [sender])
    ```
    ``` code
    (| (mail = [sender])(alternate_email = [sender]) )
    ```
    ``` code
    (| (mail = [sender])(alternate_email = [sender])(alternate  = [sender]) )
    ```

  - `email_attribute`
    The name of the attribute for the canonic email in your directory: for instance `mail`, `canonic_email`, `canonic_address`, ... In the previous example, the canonic email is `mail`.

  - `alternative_email_attribute`
    The name of the attribute for the alternate email in your directory: for instance `alternate_email`, `mailalternateaddress`, ... You make a list of these attributes separated by commas.

    With this list, Sympa creates a cookie which contains various information: whether the user is authenticated via LDAP or not, his alternate email. Storing the alternate email is interesting when you want to canonify your preferences and subscriptions, that is to say you want to use a unique address in `user_table` and `subscriber_table`, which is the canonic email.

  - `scope` (Default value: `sub`)
    By default, the search is performed on the whole tree below the specified base object. This may be changed by specifying a scope:

      - `base`: search only the base object,

      - `one`: search the entries immediately below the base object,

      - `sub`: search the whole tree below the base object. This is the default.

  - `authentication_info_url`
    Defines the URL of a document describing LDAP password management. When hitting Sympa's *Send me a password* button, LDAP users will be redirected to this URL.

Following parameters are used to provide LDAPS (LDAP over TLS/SSL):

  - `use_ssl` (OBSOLETE)
    If set to `1`, connection to the LDAP server will use LDAPS (LDAP over TLS/SSL).

      - Obsoleted as of Sympa 6.2.15. Use `use_tls` instead.

  - `use_tls` (Default value: `none`):

      - `ldaps`: use LDAPS (LDAP over TLS/SSL),

      - `starttls`: use StartTLS,

      - `none`: TLS (SSL) is disabled.

  - `ssl_version` (Default value: `tlsv1`)
    This defines the version of the TLS/SSL protocol to use. Possible values are `sslv2`, `sslv3`, `tlsv1`, `tlsv1_1` and `tlsv1_2`.

  - `ssl_ciphers`
    Specify which subset of cipher suites are permissible for this connection, using the standard OpenSSL string format. The default value of Net::LDAPS for ciphers is `ALL`, which permits all ciphers, even those that do not encrypt!

Additionally, following parameters may also used with Sympa 6.2 or later:

  - `ssl_cert`
    Path to client certificate.

  - `ssl_key`
    Path to the secret key of client certificate.

  - `ca_verify`
    `none`, `optional` or `required`. If set to `none`, will never verify server certificate. Latter two need appropriate `ca_path` and/or `ca_file` settings.

  - `ca_path`
    Path to directory store of CA certificates.

  - `ca_file`
    Path to file store of CA certificates.

### generic\_sso paragraph

  - `service_name`
    This is the SSO service name that will be offered to the user in the login banner menu.

  - `service_id`
    This service ID is used as a parameter by Sympa to refer to the SSO service (instead of the service name).
    A corresponding URL on the local web server should be protected by the SSO system; this URL would look like `http://yourhost.yourdomain/sympa/sso_login/inqueue` if the `service_id` is `inqueue`.

  - `http_header_list`
    Sympa gets user attributes from environment variables coming from the web server. These variables are then cached in the `user_table` DB table for later use in authorization scenarios (in structure). You can define a coma-separated list of header field names.

  - `http_header_prefix`
    Only environment variables starting with the defined prefix will be kept. Another option is to list HTTP header fields explicitely using `http_header_list` parameter.

  - `email_http_header`
    This parameter defines the environment variable that will contain the authenticated user's email address.

  - `http_header_value_separator` (default: ';'): user attributes may be multi-valued (including the user email address. This parameter defines the values separator character(s).

  - `logout_url`
    This optional parameter allows to specify the SSO logout URL. If defined, Sympa will redirect the user to this URL after the Sympa logout has been performed.

The following parameters define how Sympa can check the user email address, either provided by the SSO or by the user himself:

  - `internal_email_by_netid`
    If set to `1`, this parameter makes Sympa use its `netidmap` table to associate NetIDs to user email addresses.

  - `netid_http_header`
    This parameter defines the environment variable that will contain the user's identifier. This netid will then be associated with an email address provided by the user.

  - `force_email_verify`
    If set to `1`, this parameter makes Sympa check the user's email address. If the email address was not provided by the authentication module, then the user is requested to provide a valid email address.

The following parameters define how Sympa can retrieve the user email address; **these are useful only in case the `email_http_header` entry was not defined:**

  - `ldap_host`
    The LDAP host Sympa will connect to fetch user email. The `ldap_host` include the port number and it may be a comma separated list of redondant hosts.

  - `ldap_bind_dn`
    The DN used to bind to this server. Anonymous bind is used if this parameter is not defined.

  - `ldap_bind_password`
    The password used unless anonymous bind is used.

  - `ldap_suffix`
    The LDAP suffix used when searching user email.

  - `ldap_scope`
    The scope used when searching user email. Possible values are `sub`, `base` and `one`.

  - `ldap_get_email_by_uid_filter`
    The filter used to perform the email search. It can refer to any environment variables inherited from the SSO module, as shown below.

    Example:
    ``` code
    ldap_get_email_by_uid_filter (mail=[SSL_CLIENT_S_DN_Email])
    ```

  - `ldap_email_attribute`
    The attribute name to be used as user canonical email. In the current version of Sympa, only the first value returned by the LDAP server is used.

  - `ldap_timeout`
    The time out for the search.

To support LDAPS (LDAP over SSL/TLS), corresponding parameters in ldap paragraph may also be used for generic\_sso.

### cas paragraph

Note that Sympa will act as a CAS client to validate CAS tickets. During this exchange, Sympa will check the CAS server x.509 certificate. Therefore you should ensure that the certificate autority of the CAS server is known by Sympa ; this should be configured through the [cafile](/manual/conf-parameters/part3#cafile) or [capath](/manual/conf-parameters/part3#capath) sympa.conf configuration parameters.

  - `auth_service_name`
    The authentication service name. Note that it is used as an identifier in the code; it should therefore be made of alphanumeric characters only, with no space.

  - `auth_service_friendly_name`
    If defined, this string is proposed on the web login banner.

  - `host` (OBSOLETE)
    This parameter has been replaced by **base\_url** parameter

  - `base_url`
    The base URL of the CAS server.

  - `non_blocking_redirection` on | off

    Default value: `on`

    This parameter only concerns the first access to Sympa services by a user, it activates or not the non blocking redirection to the related CAS server to check automatically if the user as been previously authenticated with this CAS server. The redirection to CAS is used with the CGI parameter `gateway=1` that specifies to CAS server to always redirect the user to the original URL, but just check if the user is logged. If active, the SSO service is effective and transparent, but in case the CAS server is out of order, the access to Sympa services is impossible.

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

  - `ldap_host`
    The LDAP host Sympa will connect to fetch user email when user uid is return by CAS service. The `ldap_host` includes the port number and it may be a comma separated list of redondant hosts.

  - `ldap_bind_dn`
    The DN used to bind to this server. Anonymous bind is used if this parameter is not defined.

  - `ldap_bind_password`
    The password used unless anonymous bind is used.

  - `ldap_suffix`
    The LDAP suffix used when searching user email.

  - `ldap_scope`
    The scope used when searching user email. Possible values are `sub`, `base` and `one`.

  - `ldap_get_email_by_uid_filter`
    The filter used to perform the email search.

  - `ldap_email_attribute`
    The attribute name to be used as user canonical email. In the current version of Sympa, only the first value returned by the LDAP server is used.

  - `ldap_timeout`
    The time out for the search.

To support LDAPS (LDAP over SSL/TLS), corresponding parameters in ldap paragraph may also be used for cas.

Sharing WWSympa's authentication with other applications
--------------------------------------------------------

If you are not using a web Single Sign On system, you might want to make other web applications collaborate with Sympa and share the same authentication system. Sympa uses HTTP cookies to carry users' authentication information from page to page. This cookie contains no information about privileges. To make your application work with Sympa, you have two possibilities:

  - Delegating authentication operations to *WWSympa*

    If you want to avoid spending a lot of time programming a CGI to do Login, Logout and Remindpassword, you can copy *WWSympa*'s login page to your application, and then make use of the cookie information within your application. The cookie format is:
    ``` code
    sympauser=<user_email>:<checksum>
    ```
    where `<user_email>` is the user's complete e-mail address, and `<checksum>` represents the 8 last bytes of the MD5 checksum of the `<user_email>`+Sympa `cookie` configuration parameter. Your application needs to know what the `cookie` parameter is, so it can check the HTTP cookie validity; this is a secret shared between *WWSympa* and your application. *WWSympa*'s `loginrequest` page can be called to return to the referer URL when an action is performed. Here is a sample HTML anchor:
    ``` html
    <A HREF="/sympa/loginrequest/referer">Login page</A>
    ```
    You can also have your own HTML page submitting data to `wwsympa.fcgi` CGI. If you do so, you can set the `referer` variable to another URI. You can also set the `failure_referer` to make *WWSympa* redirect the client to a different URI if login fails.

  - Using *WWSympa*'s HTTP cookie format within your authentication module

    To cooperate with *WWSympa*, you simply need to adopt its HTTP cookie format and share the secret it uses to generate MD5 checksums, i.e. the `cookie` configuration parameter. In this way, *WWSympa* will accept users authenticated through your application without further authentication.

Perl example
------------

Here is a example using perl that show both method : use Sympa login page or copy login form into your application. You can try it on Sympa author's lists server : [http://listes.renater.fr/cgi-bin/sample.cgi](http://listes.renater.fr/cgi-bin/sample.cgi).

``` perl
#!/usr/bin/perl -U
 
## Just a example how to use Sympa Session
 
use strict;
use CGI;
use DBI;
use lib '/home/sympa/bin';
use Conf;
use SympaSession;
# use cookielib;
use List;
my $query = new CGI;
my %in = $query->Vars;
my $robot = 'renater.fr';
my $email;
 
unless (&Conf::load('/etc/sympa.conf')) {
    printf "Configuration file /etc/sympa.conf has errors.\n";
}
### this section is mandatory to have List::is_user working
if ($Conf{'db_name'} and $Conf{'db_type'}) {
    unless ($List::use_db = &Upgrade::probe_db()) {
        printf "Error could not connect to database";
    }
}
&List::_apply_defaults();
print "Content-Type: text/html\n\n";
print "<html><head><title>just a test</head><body>";
if ($ENV{'HTTP_COOKIE'} =~ /(sympa_session)\=/) {
    my  $session = new SympaSession ($robot,{'cookie'=>&SympaSession::get_session_cookie($ENV{'HTTP_COOKIE'})});
    $email = $session->{'email'} unless ($session->{'email'} eq 'nobody');
}
printf "<h1>welcome</h1>";
if ($email) {
    printf "Sympa session is recognized from cookie sympa_session. Your email is <b>%s</b>\n",$email;
}else{
print '
<h4> method 1: use Sympa form</h4>
This method is very simple, just link the Sympa login page using the <i>referer</i> parameter.
The URL look like http://mysserver.domain.org/sympa/loginrequest/referer.
<a href="/sympa/loginrequest/referer">Try it</a>
 
<h4>method 2 : copy login form into your application</h4>
  <form action="http://listes.renater.fr/sympa" method="post">
        <input type="hidden" name="referer" value="http://myserver.domain.org/cgi-bin/sample.cgi" />
        <input type="hidden" name="failure_referer" value " http://myserver.domain.org/cgi-bin/error_login.html" />
        <input type="hidden" name="action" value="login" />
        <label for="email">email address:
        <input type="text" name="email" id="email"/></label><br />
        <label for="passwd" >password:
        <input type="password" name="passwd" id="passwd" /></label> <br/>
        <input type="submit" name="action_login" value="Login" />
  </form>';
}
print '</body></hml>';
```

How to do it using PHP ?
------------------------

Chris Hastie has sumitted a [contrib for sharing Sympa sessions with PHP applications](/contribs/index#how_to_share_sympa_session_with_other_php_applications).

What about using SOAP to access Sympa sessions
----------------------------------------------

Not yet possible but of course this the best way to do it. There is a [feature request](https://sourcesup.renater.fr/tracker/index.php?func=detail&aid=4056&group_id=23&atid=170) for it.

Provide a Sympa login form in another application
-------------------------------------------------

You can easily trigger a Sympa login from another web page. The login form should look like this:

``` html
<FORM ACTION="http://listes.renater.fr/sympa" method="post">
    <input type="hidden" name="previous_action" value="arc" />
    Access web archives of list
    <select name="previous_list">
    <option value="sympa-users" >sympa-users</option>
    </select><br/>
    <input type="hidden" name="action" value="login" />
    <label for="email">email address:
    <input type="text" name="email" id="email" size="18" value="" /></label><br />
    <label for="passwd" >password:
    <input type="password" name="passwd" id="passwd" size="8" /></label> <br/>
    <input class="MainMenuLinks" type="submit" name="action_login" value="Login and access web archives" />
</FORM>
```

The example above does not only perform the login action, but also redirects the user to another Sympa page, a list web archive here. The `previous_action` and `previous_list` variables define the action that will be performed after the login is done.

Setting up a Shibboleth-enabled Sympa server
============================================

[Sympa](http://www.sympa.org) is an open source mailing list software, provided by the CRU.

[Shibboleth](http://shibboleth.internet2.edu) is an open source distributed authentication software, provided by Internet2 consortium; it implements the SAML 2.0 protocol.

Implementation in Sympa
-----------------------

Sympa has been made Shibboleth-enabled in its 4.1 release (March 2004). The implementation is wider that Shibboleth authentication and can be used to plug Sympa web interface with almost any authentication mecanism that comes with an Apache authentication module. The feature is named **generic\_sso** in Sympa documentation.

  - [Sympa documentation for generic_sso](http://www.sympa.org/manual/authentication#generic_sso_authentication)

Prerequisites
-------------

### Sympa

You'll need to have a Sympa server running. It should be a recent version (5.3 or later) because a few bugs have been fixed.

### Shibboleth Service Provider

You need to install Shibboleth SP (Service Provider) and configure it properly to interract with your favourite federation. The documentation refers to Shibboleth SP 2.4.

  - [Installing a Shibboleth Service Provider](https://spaces.internet2.edu/display/SHIB2/NativeSPLinuxInstall)

  - [Installer un fournisseur de services Shibboleth](https://federation.renater.fr/docs/installation-sp)

How it works
------------

Sympa expects Shibboleth to provide some user informations, especially the user email address, once the user has authenticated. Sympa will get user attributes from Shibboleth Apache module via environment variables. You'll have to tell Sympa the names of these environment variables.

To keep part of Sympa web interface accessible to unauthenticated users, not all Sympa URLs are Shibboleth-protected. Only one URL needs to be protected ; Sympa will manage to collect user attributes when the user is redirected to this URL and will keep them in cache for later use, during the session.

Note that Sympa does not use lazy sessions mecanism (provided by Shibboleth); the mecanism used almost provides the same level of flexibility and can be adapted to authentication mecanisms other than Shibboleth.

Configuring Sympa
-----------------

Sympa gathers all web authentication configuration in a single file (can have one per virtual host): **auth.conf**. You should create/edit this configuration file to add the Shibboleth-related part. Here is a sample */home/sympa/etc/auth.conf* file:

``` code
generic_sso
    service_name       Connexion
    service_id         federation_renater
    http_header_list mail
    email_http_header  mail
    logout_url         https://lists.your.domain/Shibboleth.sso/Logout?return=https%3A%2F%2Flists.your.domain/sympa

#user_table
#    negative_regexp                 .*
```

Note that:

  - service\_name (*Connexion* here) is the name of the button (of menu item) for logging in, in Sympa web interface;

  - service\_id (*federation\_renater* here) is the identifier of this authentication server within Sympa. You'll notice later that login URLs in Sympa will include this token;

  - http\_header\_prefix is of no use with Shibboleth SP 2.x because user attributes provided by Apache no more share a common prefix (as Shibboleth 1.3 used to do). Set this parameter to the same value as the *email\_http\_header* parameter. Note that Sympa 6 includes a new *http\_header\_list* more adapted to declare the set of user attributes that are worth getting from Shibboleth.

  - email\_http\_header is the name of the environment variable that brings the user email address. It used to be *HTTP\_SHIB\_INETORGPERSON\_MAIL* with Shibboleth 1.3 and the default with Shibboleth 2.x is *mail*. Note that until Sympa 6, multi-valued are not properly handled by Sympa. Sympa 6 provides a new *http\_header\_value\_separator* with a default value of *;*.

  - logout\_url is the URL the user will be redirected to for logging out. Note the *return* parameter provided here to make the user come back to Sympa web interface after logout is completed.

  - user\_table authentication method is disabled, thus providing Shibboleth authentication ONLY; the standard Sympa login banner will disappear from the web interface.

You'll need to restart your Apache server to make Sympa web server take these changes into account:

``` code
/etc/init.d/httpd restart
```

You'll note the new "Connexion" button on your Sympa web interface:

[shiblogin.png: Work in progress]

Configuring Apache
------------------

Shibboleth authentication needs to be triggered on a dedicated URL. Here is a sample Apache configuration:

``` code
<Location /sympa/sso_login/federation_renater>
    AuthType shibboleth
    ShibRequestSetting requireSession true
    ShibRequestSetting applicationId app-sympa
    require shibboleth
#    require mail ~ @
</Location>
```

Note that:

  - the protected URL includes the *service\_id* (here *federation\_renater*); you should replace with the federation\_id you defined in your *auth.conf* file.

  - ShibApplicationID directive reffers to the application context you'll define in Shibboleth configuration file.

  - we could have used the *require mail* directive (commented here) to enforce that the remote Identity Provider provides the user email address; we did not, to let Sympa cope with errors more smoothly than Shibboleth SP does.

Now restart your web server to validate these changes:

``` code
/etc/init.d/httpd restart
```

You can have a try accessing `http://lists.your.domain/sympa/sso_login/federation_renater`; it should trigger the Shibboleth user authentication.

Configuring Shibboleth
----------------------

Shibboleth SP's main configuration file is **/etc/shibboleth/shibboleth2.xml**. That's the only configuration file you'll have to edit to make Shibboleth authentication work with Sympa.

You'll need to define an application context specific for your Sympa service. If you already have a Shibboleth-enabled service running on the server, you should either using the same application context for Sympa or define a new one using the *ApplicationOverride* directive.

Here is a sample piece of *shibboleth2.xml*:

``` xml
<ApplicationOverride id="app-sympa" entityID="https://lists.your.domain/sympa">
 
    <Sessions lifetime="28800" timeout="3600" checkAddress="false" handlerSSL="true">
 
        <SSO discoveryProtocol="SAMLDS" discoveryURL="https://services-federation.renater.fr/test/wayf">
            SAML2 SAML1
        </SSO>
 
    </Sessions>
</ApplicationOverride>
```

Note that:

  - the *ApplicationOverride/id* attribute needs to be the same you defined in your Apache *ShibApplicationID* directive value. An alternative is to use Shibboleth RequestMapper but it's much harder to configure;

  - the value of *ApplicationOverride/entityID* is your Sympa service entityID, used later to declare the service at your favourite federation;

Restart your Apache server and shibd daemon to use the new configuration file :

``` code
/etc/init.d/https restart
/etc/init.d/shibd restart
```

Declaring your Sympa service in your favourite federation
---------------------------------------------------------

It's now time to let identity providers know about your federated mailing list service, otherwise they will not authenticate users that come from your service, or provide no user attributes. You favourite federation probably provides a web form to declare federated resources.

  - [resource registration for French users](https://services-federation.renater.fr/gestion?action=get_all&amp;federation=renater)

Before you register your mailing list in a production federation you should do some testing. You can either test within a Test federation ([see the French test federation](https://federation.renater.fr/en/test-federation)) or setup a bilateral trust relationship with a single Identity Provider ([documentation in French to setup a bilateral relationship](https://federation.renater.fr/docs/fiches/fedadeux)).

You'll need to make sure all Identity Providers will provide the user email address; therefore they'll need to configure their *attribute release policy*.

Coping with virtual hosts
-------------------------

If you have Sympa virtual robots for other virtual hosts, you'll need to define distinct ApplicationOverride Shibboleth configuration elements for each virtual host. You'll have to declare each of them to your favourite federation, since they appear as separate services.

What if you don't trust provided email addresses?
-------------------------------------------------

You might not trust the email address user attribute provided by some Identity Providers, because the provisioning of this attribute is too weak (provided by the user himself, without further checks). This raises security issue because anybody who logs in with somebody else's email address gets this person's privileges on the mailing list server.

This issue can be addressed by Sympa thanks to an extension developped by JP Robinson that allows Sympa to either validate the user's email address or even collect it (later associated to a Shibboleth user identifier).

Please read the documentation for further informations:
  * [generic_sso documentation](http://www.sympa.org/manual/authentication#generic_sso_paragraph)

