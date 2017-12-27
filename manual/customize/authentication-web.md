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
    service_name                InQueue Federation
    service_id                  inqueue
    http_header_list            mail,displayName,eduPersonAffiliation
    email_http_header           mail

## The email address is not provided by the user home institution
generic_sso
    service_name                Shibboleth Federation
    service_id                  myfederation
    http_header_list            mail,displayName,eduPersonAffiliation
    netid_http_header           mail
    internal_email_by_netid     1
    force_email_verify          1

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

user_table
    negative_regexp             ((univ-rennes1)|(univ-nancy2))\.fr
```

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

