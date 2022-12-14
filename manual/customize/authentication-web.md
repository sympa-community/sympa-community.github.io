---
title: 'Authentication on web interface'
up: ../customize.md#customizing-web-interface
---

Authentication on web interface
===============================

Sympa needs to authenticate users (subscribers, owners, moderators, listmasters) on both its mail and web interface, and then apply appropriate privileges (authorization process) to subsequent requested actions. Sympa is able to cope with multiple authentication means on the client side, and when using account with user and password, it can validate these credentials against authentication backends.

Authentication mechanisms
-------------------------

On the web interface (*WWSympa*), the user can authenticate in four different
**authentication mechanisms** (if appropriate setup has been done on the Sympa
server):

  - Default authentication mechanism is performed through the user's email
    address and a password managed by Sympa itself.

  - If one or more LDAP authentication backends has been defined, then the
    user can authenticate with their LDAP uid and password.

  - Sympa is also able to delegate the authentication process to a web single
    sign-on (SSO) system. Currently following systems are supported:

      - [CAS](https://developers.yale.edu/documentation/Administrative/cas)
        (the Yale University system).
      - A generic SSO setup, adapted to SSO products providing the module for
        Apache HTTP Server.

  - External authentication mechanism may be used through the Common Gateway
    Interface (CGI).

  - When contacted via HTTPS, Sympa can make use of X.509 client certificates
    to authenticate users.

The authorization process in Sympa
([authorization scenarios](basics-scenarios.md)) refers to
**authentication methods**. The same authorization scenarios are used for both
mail and web accesses; therefore some authentication methods are considered
to be equivalent: See
"[Authentication methods](basics-scenarios.md#authentication-methods)".

### Built-in authentication mechanism

By default, only this mechanism is enabled.

Sympa stores the data relative to users in the
[`user_table`](/gpldoc/man/sympa_database.5.html#user_table) database table.
Among these data, password and email address are used during the web
authentication. 

Built-in authentication mechanism is described by
[`user_table`](/gpldoc/man/auth.conf.5.html#user_table-paragraph) paragraph in
[`auth.conf`](/gpldoc/man/auth.conf.5.html) configuration file.  Below is an example
of the paragraph:
```code
user_table
    negative_regexp             ((univ-rennes1)|(univ-nancy2))\.fr
```

See "[Built-in authentication](builtin-auth.md)" for more details.

### LDAP authentication

Authentication based on information stored in LDAP directory.  This mechanism
may be defined by [`ldap`](/gpldoc/man/auth.conf.5.html#ldap-paragraph) paragraph in
`auth.conf` configuration file.  See "[LDAP authentication](ldap-auth.md)"
for more details.

### CAS single sign-on

See "[CAS single sign-on](cas.md)" for details.

### Generic SSO authentication

This authentication mechanism has first been introduced to allow interaction with [Shibboleth](http://shibboleth.internet2.edu/), Internet2's inter-institutional authentication system. But it should be usable with any SSO system that provides an Apache HTTP Server's authentication module being able to protect a specified URL on the site (not the whole site).

Sympa will get user attributes via environment variables. In the most simple case, the SSO will provide the user email address. If not, Sympa can be configured to check an email address provided by the user, or to look for the user email address in a LDAP directory (the search filter will make use of user information inherited from the SSO Apache module).

Apart from the user email address, the SSO can provide other user attributes that Sympa will store in the [`user_table`](/gpldoc/man/sympa_database.5.html#user_table) database table (for persistancy), and make available in the `[user_attributes->`*keyword*`]` variable that you can use within [authorization scenarios](basics-scenarios.md) or in [web templates](basics-templates.md#mail-and-web-template files) via the `[%user.attributes.`*keyword*`%]` variable.

To plug a new SSO server in your Sympa server, you should add a [`generic_sso`](/gpldoc/man/auth.conf.5.html#generic_sso-paragraph) paragraph describing the SSO service in your `auth.conf` configuration file.
Once this paragraph has been added, the SSO service name will be automatically added to the web login menu.

Below is an example of the paragraph:

``` code
## The URL corresponding to the service_id should be protected by the SSO (Shibboleth in the example)
## The URL would look like http://yourhost.yourdomain/sympa/sso_login/inqueue in
 the following example
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
```

And here is a sample `httpd.conf` that Shibboleth authentication protects the associated Sympa URL:

``` code
...
<Location /sympa/sso_login/inqueue>
    AuthType shibboleth
    require mail ~ @
</Location>
...
```

For detailed description to integrate Sympa into Shibboleth and its
federation, see
"[Setting up a Shibboleth-enabled Sympa server](shibboleth.md)".

### Using external authentication mechanisms through the Common Gateway Intertface (CGI)

> **Note**
>   - CGI support for authentication is available on Sympa 6.2.71b.1 or later.

_Documentation work in progress._

### TLS client authentication

See "[TLS client authentication](tls-client-auth.md) for details.

Sympa configuration overview
----------------------------

### Location

The [`auth.conf`](/gpldoc/man/auth.conf.5.html) configuration file contains numerous
parameters which are read on start-up of Sympa.  Its location is:

  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`*mail domain*`/auth.conf`
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/auth.conf`
  - [``$DEFAULTDIR``](../layout.md#defaultdir)`/auth.conf`
    --- Distribution default.

See also "[Configuration files](basics-configuration.md#configuration-files).

> **Note**
>
>   * If you change this file, do not forget that you will need to restart
>     web interface afterwards.  See also
>     "[Reloading WWSympa](../admin/services.md#reloading-wwsympa).

### Syntax

> **Note**
>
>   * See [`auth.conf(5)`](/gpldoc/man/auth.conf.5.html) manual page for detailed
>     definition on content of `auth.conf` file.

The `auth.conf` is organized in paragraphs. Each paragraph describes an authentication service with all parameters required to perform an authentication using this service. Sympa's current version can perform authentication through LDAP directories, using an external Single Sign-On Service (like CAS or Shibboleth), or using the internal `user_table` table.

### `regexp` and `negative_regexp`: The `auth.conf` switches

Suppose your organization use two domains for its email addresses, for example "`student.univ.edu`" and "`univ.edu`". The first domain correspond to people stored in a part of the LDAP directory , and the other one to people stored in another part. You may want to define a specific authentication paragraph for each of these groups.

the `regexp` subparameter --- and its evil twin `negative_regexp` --- are exactly used to perform such a distinction: it allows you to apply different authentication paragraph based on which email address was provided by the user. Let's emphasize this: **The `regexp` and `negative_regexp` are applied to email addresses *only*. It will *not* work on user ID.**

Each paragraph in `auth.conf` can contain an occurence of these subparameters. Their value is a regexp (just the regexp part. No delimiters).

Example:

``` code
regexp         student\.univ\.edu
```

What do they do?

They are tested among the domain part of the email provided. The paragraph will be used with this email address if it matches the expression defined by `regexp` or if it does *not* match the expression defined by `negative_regexp`.

### Login form

The login page contains two forms: the login form and the SSO form. When users hit the login form, each `ldap` or `user_table` authentication paragraph is applied, unless email adress input from form matches the `negative_regexp` or do not match `regexp`. `negative_regexp` and `regexp` can be defined for each `ldap` or `user_table` authentication service so that administrators can block some authemtication mechanisms for a class of users.

The second form in the login page contains the list of CAS servers so that users can choose explicitly their CAS service.

Sharing WWSympa's authentication with other applications
--------------------------------------------------------

(Work in progress)

### Perl example

Here is a example using perl that show both method : use Sympa login page or copy login form into your application. You can try it on Sympa author's lists server : [http://listes.renater.fr/cgi-bin/sample.cgi](http://listes.renater.fr/cgi-bin/sample.cgi).

``` perl
#!/usr/bin/perl

## Just a example how to use Sympa Session.

use strict;
use warnings;
use lib '$LIBDIR';    # Replace with actual path.

use Conf;
use Sympa::DatabaseManager;
use Sympa::Session;

my $robot = 'mail.domain.org';    # Replace with actual mail domain.
my $email;

unless (Conf::load()) {
    die "Configuration file sympa.conf has errors.\n";
}
unless (Sympa::DatabaseManager->instance) {
    die "Error could not connect to database";
}

if ($ENV{HTTP_COOKIE} =~ /(sympa_session)=/) {
    my $session = Sympa::Session->new($robot,
        {cookie => Sympa::Session::get_session_cookie($ENV{HTTP_COOKIE})});
    $email = $session->{email}
        if $session->{email} and $session->{email} ne 'nobody';
}

print "Content-Type: text/html\n\n";
print '<html><head><title>Just a test</head><body>';

if ($email) {

    printf <<"EOF";
<h1>Welcome</h1>

<p>
  Sympa session is recognized from cookie sympa_session.
</p>
<p>
  Your email is <strong>$email</strong>.
</p>
EOF

} else {

    print <<'EOF';
<h1> method 1: Use Sympa form</h1>

<p>
  This method is very simple, just link the Sympa login page using the
  <i>referer</i> parameter.
  The URL look like http://myserver.example.org/sympa/loginrequest/referer.
</p>
<p>
  <a href="/sympa/loginrequest/referer">Try it</a>
</p>

<h1>method 2: Copy login form into your application</h1>

<form action="http://listes.renater.fr/sympa" method="POST">
  <input type="hidden" name="referer"
   value="http://myserver.example.org/cgi-bin/sample.cgi" />
  <input type="hidden" name="failure_referer"
   value="http://myserver.example.org/cgi-bin/error_login.html" />
  <input type="hidden" name="action" value="login" />
  <p>
    <label for="email">email address:</label>
    <input type="text" name="email" id="email" />
  <p>
    <label for="passwd" >password:</label>
    <input type="password" name="passwd" id="passwd" />
  </p>
  <p>
    <input type="submit" name="action_login" value="Login" />
  </p>
</form>
EOF

}

print '</body></html>';
```

### How to do it using PHP ?

Chris Hastie has sumitted a [contrib for sharing Sympa sessions with PHP applications](https://www.sympa.org/contribs/index#how_to_share_sympa_session_with_other_php_applications).

<!--
### What about using SOAP/HTTP API to access Sympa sessions

Not yet possible but of course this the best way to do it. There is a [feature request](https://sourcesup.renater.fr/tracker/index.php?func=detail&aid=4056&group_id=23&atid=170) for it.
-->

Provide a Sympa login form in another application
-------------------------------------------------

You can easily trigger a Sympa login from another web page. The login form should look like this:

``` html
<form action="http://listes.renater.fr/sympa" method="POST">
  <input type="hidden" name="previous_action" value="arc" />
  <p>
    Access web archives of list
    <select name="previous_list">
      <option value="sympa-users" >sympa-users</option>
    </select>
  </p>
  <input type="hidden" name="action" value="login" />
  <p>
    <label for="email">email address:</label>
    <input type="text" name="email" id="email" size="18" />
  </p>
  <p>
    <label for="passwd" >password:</label>
    <input type="password" name="passwd" id="passwd" size="8" />
  </p>
  <p>
    <input class="MainMenuLinks" type="submit" name="action_login"
     value="Login and access web archives" />
  </p>
</form>
```

The example above does not only perform the login action, but also redirects the user to another Sympa page, a list web archive here. The `previous_action` and `previous_list` variables define the action that will be performed after the login is done.

