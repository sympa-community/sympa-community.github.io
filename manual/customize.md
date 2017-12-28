---
title: 'Customizing Sympa'
prev: admin.md
up: ./
---

Customizing Sympa
=================

----
Note:

  * This chapter is work in progress.

    Please consider adding description and instruction written by your own
    (See [CONTRIBUTING](../CONTRIBUTING.md)).

----

Customization basics
--------------------

  * [Message workflow](customize/basics-workflow.md)
  * [Roles and privileges](customize/basics-roles.md)
  * [Configuration hierarchy](customize/basics-configuration.md)
  * [Templates](customize/basics-templates.md)
  * [Authorization scenarios](customize/basics-scenarios.md)
  * [Tasks](customize/basics-tasks.md)
  * [Internationalization](customize/basics-i18n.md)
  * [List families](customize/basics-families.md)

Customizing Sympa services
--------------------------

----
Note:

  * See related links of each feature:
      - &#x1F527; for configuration parameters,
      - &#x2699; for configuration files and
      - &#x1F4E6; for optional external modules.

----

To manage these features, listmaster privileges and/or console login are
needed.

  * Receiving
    [&#x1F527;](man/sympa.conf.5.md#receiving)
  * Sending related
    [&#x1F527;](man/sympa.conf.5.md#sending-related)
    [&#x2699;](man/nrcpt_by_domain.conf.5.md "nrcpt_by_domain.conf")
  * Privileges
    [&#x1F527;](man/sympa.conf.5.md#privileges)
    [&#x2699;](man/edit_list.conf.5.md "edit_list.conf")
  * Archives
    [&#x1F527;](man/sympa.conf.5.md#archives)
    [&#x2699;](man/mhonarc_ressources.tt2.5.md "mhonarc_ressources.tt2")
  * [Bounce management](customize/bounce-management.md)
    [&#x1F527;](man/sympa.conf.5.md#bounce-management-and-tracking)
  * Loop prevention
    [&#x1F527;](man/sympa.conf.5.md#loop-prevention)
  * [Automatic list creation](customize/automatic-lists.md)
    [&#x1F527;](man/sympa.conf.5.md#automatic-lists)
  * Tag based spam filtering
    [&#x1F527;](man/sympa.conf.5.md#tag-based-spam-filtering)
  * Directories
    [&#x1F527;](man/sympa.conf.5.md#directories)
  * Miscelaneous
    [&#x1F527;](man/sympa.conf.5.md#miscelaneous)
  * Others

      - [Custom list parameters](customize/custom-parameters.md)
        [&#x1F527;](man/list_config.5.md#custom_vars)
      - [Custom user attributes](customize/custom-user-attributes.md)
        [&#x1F527;](man/list_config.5.md#custom_attribute)

      - Custom user attributes in database
        [&#x1F527;](man/sympa.conf.5.md#db_additional_subscriber_fields)
        [&#x1F527;](man/sympa.conf.5.md#db_additional_user_fields)

Customizing web interface
--------------------------

To manage these features, listmaster privileges and/or console login are
needed.

  * Appearances
    [&#x1F527;](man/sympa.conf.5.md#web-interface-parameters-appearances)
  * [Authentication on web interface](customize/authentication-web.md)
    [&#x2699;](man/auth.conf.5.md "auth.conf")
  * Miscelaneous
    [&#x1F527;](man/sympa.conf.5.md#web-interface-parameters-miscelaneous)

      - Session and cookie
        [&#x1F527;](man/sympa.conf.5.md#cookie_domain)
      - Shared document repository
        [&#x1F527;](man/sympa.conf.5.md#default_shared_quota)
      - HTML editor
        [&#x1F527;](man/sympa.conf.5.md#use_html_editor)
      - Password
        [&#x1F527;](man/sympa.conf.5.md#max_wrong_password)
      - One time ticket
        [&#x1F527;](man/sympa.conf.5.md#one_time_ticket_lifetime)
      - Pictures
        [&#x1F527;](man/sympa.conf.5.md#pictures_feature)
      - Protection against spam harvesters
        [&#x1F527;](man/sympa.conf.5.md#spam_protection)
        [&#x2699;](man/crawlers_detection.conf.5.md "crawlers_detection.conf")

  * [Message tracking](customize/message-tracking.md)
    [&#x1F527;](man/sympa.conf.5.md#bounce-management-and-tracking)
  * [RSS feed](customize/rss-feed.md)
  * [User-friendly automatic lists](customize/friendly-automatic-lists.md)
    [&#x1F527;](man/sympa.conf.5.md#automatic_list_families)
    [&#x2699;](man/automatic_lists_description.conf.5.md "automatic_lists_description.conf")

Sympa services: Optional features
---------------------------------

These features need installation of additional software components including
external Perl modules.

  * S/MIME
    [&#x1F527;](man/sympa.conf.5.md#s-mime-and-tls)
    [&#x1F4E6;](https://metacpan.org/release/Crypt-OpenSSL-X509 "Crypt-OpenSSL-X509")
    [&#x1F4E6;](https://metacpan.org/release/Crypt-SMIME "Crypt-SMIME")
  * Data sources setup
    [&#x1F527;](man/sympa.conf.5.md#data-sources-setup)
    [&#x1F4E6;](https://metacpan.org/release/DBD-CSV "DBD-CSV")
    [&#x1F4E6;](https://metacpan.org/release/DBD-mysql "DBD-mysql")
    [&#x1F4E6;](https://metacpan.org/release/DBD-ODBC "DBD-ODBC")
    [&#x1F4E6;](https://metacpan.org/release/DBD-Oracle "DBD-Oracle")
    [&#x1F4E6;](https://metacpan.org/release/DBD-Pg "DBD-Pg")
    [&#x1F4E6;](https://metacpan.org/release/DBD-SQLite "DBD-SQLite")
    [&#x1F4E6;](https://metacpan.org/release/DBD-Sybase "DBD-Sybase")
    [&#x1F4E6;](https://metacpan.org/release/Net-LDAP "Net-LDAP")
    [&#x1F4E6;](https://metacpan.org/release/IO-Socket-SSL "IO-Socket-SSL")
  * [DKIM](customize/dkim.md)
    [&#x1F527;](man/sympa.conf.5.md#dkim)
    [&#x1F4E6;](https://metacpan.org/release/Mail-DKIM "Mail-DKIM")
  * [DMARC protection](customize/dmarc-protection.md)
    [&#x1F527;](man/sympa.conf.5.md#dmarc-protection)
    [&#x1F4E6;](https://metacpan.org/release/Net-DNS "Net-DNS")
  * Managing aliases with LDAP
    [&#x2699;](man/ldap_alias_manager.conf.5.md "ldap_alias_manager.conf")
    [&#x2699;](man/ldap_alias_entry.tt2.5.md "ldap_alias_entry.tt2")
    [&#x1F4E6;](https://metacpan.org/release/Net-LDAP "Net-LDAP")
    [&#x1F4E6;](https://metacpan.org/release/IO-Socket-SSL "IO-Socket-SSL")
  * ~~Managing aliases with RDBMS~~ (currently broken)
  * List address verification
    [&#x1F527;](man/sympa.conf.5.md#list-address-verification)
    [&#x1F4E6;](https://metacpan.org/release/libnet "libnet")
    ([&#x1F4E6;](https://metacpan.org/pod/Net::SMTP "Net::SMTP"))
  * [Antivirus plug-in](customize/antivirus.md)

  * Miscelaneous

    &#x1F4E6; Optionally [Clone](https://metacpan.org/release/Clone),
    [Encode-Locale](https://metacpan.org/release/Encode-Locale) etc.

Web interface: Optional features
--------------------------------

These features need installation of additional software components including
external Perl modules.

  * [CAS single sign-on](customize/cas.md)
    [&#x2699;](man/auth.conf.5.md#cas-paragraph "auth.conf")
    [&#x1F4E6;](https://metacpan.org/release/AuthCAS "AuthCAS")
  * [TLS client authentication](customize/tls-client-auth.md)
    [&#x1F4E6;](https://metacpan.org/release/Crypt-OpenSSL-X509 "Crypt-OpenSSL-X509")
  * Password validation
    [&#x1F527;](man/sympa.conf.5.md#password-validation)
    [&#x1F4E6;](https://metacpan.org/release/Data-Password "Data-Password")
  * [Authentication with LDAP](customize/ldap-auth.md)
    [&#x2699;](man/auth.conf.5.md#ldap-paragraph "auth.conf")
    [&#x1F4E6;](https://metacpan.org/release/Net-LDAP "Net-LDAP")
    [&#x1F4E6;](https://metacpan.org/release/IO-Socket-SSL "IO-Socket-SSL")
  * [Setting up a Shibboleth-enabled Sympa server](customize/shibboleth.md)
    [&#x2699;](man/auth.conf.5.md#generic_sso-paragraph "auth.conf")
    [&#x1F4E6;](http://shibboleth.internet2.edu "Shibboleth SP")
  * [SOAP/HTTP API](customize/soap-api.md)
    [&#x1F527;](man/sympa.conf.5.md#soap-http-interface)
    [&#x2699;](man/trusted_applications.conf.5.md "trusted_applications.conf")
    [&#x2699;](man/sympa.wsdl.5.md "sympa.wsdl")
    [&#x1F4E6;](https://metacpan.org/release/SOAP-Lite "SOAP-Lite")

  * Miscelaneous

    &#x1F4E6; Optionally
    [Crypt-CipherSaber](https://metacpan.org/release/Crypt-CipherSaber),
    [Unicode-Nomalize](https://metacpan.org/release/Unicode-Nomalize).

Extending Sympa
---------------

These features need skill of Perl programming.

  - [Custom scenario conditions](customize/custom-scenario-conditions.md)
  - [Message hooks](man/Sympa-Message-Plugin.3.md)
  - [Template plugins](customize/template-plugins.md)

      - [List of template plugins](customize/template-plugins.md#list-of-template-plugins)

  - [Custom actions for web interface](customize/custom-actions.md)

Sympa and other systems
-----------------------

### Integrating Sympa with other systems

  - [LMTP delivery](customize/lmtp-delivery.md)
  - ~~OpenSocial integration~~ (under development)
  - Placing Sympa behind mail relay
  - Placing WWSympa behind reverse proxy
  - Sympa high availability (HA) cluster

### Other topics

  - Migrating from other mailing list systems

