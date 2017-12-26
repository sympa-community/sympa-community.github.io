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

