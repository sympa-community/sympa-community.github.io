---
title: 'Customizing Sympa'
prev: admin.md
up: ./
---

Customizing Sympa
=================

----
Notes:

  * This chapter is work in progress.

    Please consider adding description and instruction written by your own
    (See [CONTRIBUTING](../CONTRIBUTING.md)).

  * See related links of each feature:
      - &#x1F527; for configuration parameters,
      - &#x2699; for configuration files and
      - &#x1F4E6; for optional external modules.

----

Customization basics
--------------------

  * [Message workflow](customize/basics-workflow.md)
  * [Roles and privileges](customize/basics-roles.md)
  * [Configuration hierarchy](customize/basics-configuration.md)
  * [Configuration for each list](customize/basics-list-config.md)
  * [Templates](customize/basics-templates.md)
  * [Authorization scenarios](customize/basics-scenarios.md)
  * [Tasks](customize/basics-tasks.md)
  * [Internationalization](customize/basics-i18n.md)
  * [List families](customize/basics-families.md)

Customizing Sympa services
--------------------------

To manage these features, listmaster privileges and/or console login are
needed.

  * Receiving
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#receiving)
  * Sending related
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#sending-related)
    [&#x2699;](/gpldoc/man/nrcpt_by_domain.conf.5.html# "nrcpt_by_domain.conf")
  * Privileges
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#privileges)
    [&#x2699;](/gpldoc/man/edit_list.conf.5.html# "edit_list.conf")
  * [Archives](customize/archives.md)
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#archives)
    [&#x2699;](/gpldoc/man/mhonarc-ressources.tt2.5.html# "mhonarc-ressources.tt2")
  * [Bounce management](customize/bounce-management.md)
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#bounce-management-and-tracking)
  * Loop prevention
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#loop-prevention)
  * [Automatic list creation](customize/automatic-lists.md)
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#automatic-lists)
  * Tag based spam filtering
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#tag-based-spam-filtering)
  * Directories
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#directories)
  * Miscellaneous
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#miscellaneous)
  * Others

      - [Custom list parameters](customize/custom-parameters.md)
        [&#x1F527;](/gpldoc/man/list_config.5.html#custom_vars)
      - [Custom user attributes](customize/custom-user-attributes.md)
        [&#x1F527;](/gpldoc/man/list_config.5.html#custom_attribute)

      - Custom user attributes in database
        [&#x1F527;](/gpldoc/man/sympa.conf.5.html#db_additional_subscriber_fields)
        [&#x1F527;](/gpldoc/man/sympa.conf.5.html#db_additional_user_fields)

Customizing web interface
--------------------------

To manage these features, listmaster privileges and/or console login are
needed.

  * Appearances
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#web-interface-parameters-appearances)
  * [Authentication on web interface](customize/authentication-web.md)
    [&#x2699;](/gpldoc/man/auth.conf.5.html# "auth.conf")
  * Session and cookie
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#cookie_domain)
  * ~~[Shared document repository](customize/shared-repository.md)~~
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#default_shared_quota)
    (Work in progress)
  * ~~[Web mailer](customize/web-mailer.md)~~
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#use_html_editor)
    (Work in progress)
  * [Password](customize/builtin-auth.md)
    [&#x2699;](/gpldoc/man/auth.conf.5.html#user_table-paragraph "auth.conf")
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#max_wrong_password)
    [&#x1F4E6;](http://search.cpan.org/dist/Crypt-Eksblowfish/)
  * Miscellaneous
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#web-interface-parameters-miscellaneous)

      - One time ticket
        [&#x1F527;](/gpldoc/man/sympa.conf.5.html#one_time_ticket_lifetime)
      - Pictures
        [&#x1F527;](/gpldoc/man/sympa.conf.5.html#pictures_feature)
      - Protection against spam harvesters
        [&#x1F527;](/gpldoc/man/sympa.conf.5.html#spam_protection)
        [&#x2699;](/gpldoc/man/crawlers_detection.conf.5.html# "crawlers_detection.conf")

  * [Message tracking](customize/message-tracking.md)
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#bounce-management-and-tracking)
  * [RSS feed](customize/rss-feed.md)
  * [User-friendly automatic lists](customize/friendly-automatic-lists.md)
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#automatic_list_families)
    [&#x2699;](/gpldoc/man/automatic_lists_description.conf.5.html# "automatic_lists_description.conf")

Sympa services: Optional features
---------------------------------

These features need installation of additional software components including
external Perl modules.

  * [S/MIME](customize/smime.md)
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#s-mime-and-tls)
    [&#x1F4E6;](https://metacpan.org/release/Crypt-OpenSSL-X509 "Crypt-OpenSSL-X509")
    [&#x1F4E6;](https://metacpan.org/release/Crypt-SMIME "Crypt-SMIME")
  * [Data sources](customize/data-sources.md)
    [&#x1F527;](/gpldoc/man/list_config.5.html#data-sources-setup)
    [&#x1F4E6;](https://metacpan.org/release/DBD-CSV "DBD-CSV")
    [&#x1F4E6;](https://metacpan.org/release/DBD-mysql "DBD-mysql")
    [&#x1F4E6;](https://metacpan.org/release/DBD-ODBC "DBD-ODBC")
    [&#x1F4E6;](https://metacpan.org/release/DBD-Oracle "DBD-Oracle")
    [&#x1F4E6;](https://metacpan.org/release/DBD-Pg "DBD-Pg")
    [&#x1F4E6;](https://metacpan.org/release/DBD-SQLite "DBD-SQLite")
    [&#x1F4E6;](https://metacpan.org/release/Net-LDAP "Net-LDAP")
    [&#x1F4E6;](https://metacpan.org/release/IO-Socket-SSL "IO-Socket-SSL")
  * [DKIM and ARC](customize/dkim-arc.md)
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#dkim-and-arc)
    [&#x1F4E6;](https://metacpan.org/release/Mail-DKIM "Mail-DKIM")
  * [DMARC protection](customize/dmarc-protection.md)
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#dmarc-protection)
    [&#x1F4E6;](https://metacpan.org/release/Net-DNS "Net-DNS")
  * Managing aliases with LDAP
    [&#x2699;](/gpldoc/man/ldap_alias_manager.conf.5.html# "ldap_alias_manager.conf")
    [&#x2699;](/gpldoc/man/ldap_alias_entry.tt2.5.html# "ldap_alias_entry.tt2")
    [&#x1F4E6;](https://metacpan.org/release/Net-LDAP "Net-LDAP")
    [&#x1F4E6;](https://metacpan.org/release/IO-Socket-SSL "IO-Socket-SSL")
  * ~~Managing aliases with RDBMS~~ (currently broken)
  * List address verification
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#list-address-verification)
    [&#x1F4E6;](https://metacpan.org/release/libnet "libnet")
    ([&#x1F4E6;](https://metacpan.org/pod/Net::SMTP "Net::SMTP"))
  * [Antivirus plug-in](customize/antivirus.md)

  * [Miscellaneous modules](customize/misc-sympa.md)
    [&#x1F4E6;](https://metacpan.org/release/Clone)
    [&#x1F4E6;](https://metacpan.org/release/Encode-Locale)
    ...

Web interface: Optional features
--------------------------------

These features need installation of additional software components including
external Perl modules.

  * [CAS single sign-on](customize/cas.md)
    [&#x2699;](/gpldoc/man/auth.conf.5.html#cas-paragraph "auth.conf")
    [&#x1F4E6;](https://metacpan.org/release/AuthCAS "AuthCAS")
  * [TLS client authentication](customize/tls-client-auth.md)
    [&#x1F4E6;](https://metacpan.org/release/Crypt-OpenSSL-X509 "Crypt-OpenSSL-X509")
  * Password validation
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#password-validation)
    [&#x1F4E6;](https://metacpan.org/release/Data-Password "Data-Password")
  * [Authentication with LDAP](customize/ldap-auth.md)
    [&#x2699;](/gpldoc/man/auth.conf.5.html#ldap-paragraph "auth.conf")
    [&#x1F4E6;](https://metacpan.org/release/Net-LDAP "Net-LDAP")
    [&#x1F4E6;](https://metacpan.org/release/IO-Socket-SSL "IO-Socket-SSL")
  * [Setting up a Shibboleth-enabled Sympa server](customize/shibboleth.md)
    [&#x2699;](/gpldoc/man/auth.conf.5.html#generic_sso-paragraph "auth.conf")
    [&#x1F4E6;](http://shibboleth.internet2.edu "Shibboleth SP")
  * [SOAP/HTTP API](customize/soap-api.md)
    [&#x1F527;](/gpldoc/man/sympa.conf.5.html#soap-http-interface)
    [&#x2699;](/gpldoc/man/trusted_applications.conf.5.html# "trusted_applications.conf")
    [&#x2699;](/gpldoc/man/sympa.wsdl.5.html# "sympa.wsdl")
    [&#x1F4E6;](https://metacpan.org/release/SOAP-Lite "SOAP-Lite")

  * [Miscellaneous modules](customize/misc-web.md)
    [&#x1F4E6;](https://metacpan.org/release/Unicode-Nomalize)
    [&#x1F4E6;](https://metacpan.org/release/Crypt-CipherSaber)
    ...

Extending Sympa
---------------

These features need skill of Perl programming.

  - [Custom scenario conditions](customize/custom-scenario-conditions.md)
  - [Message hooks](/gpldoc/man/Sympa-Message-Plugin.3.html)
  - [Template plugins](customize/template-plugins.md)

      - [List of template plugins](customize/template-plugins.md#list-of-template-plugins)

  - [Custom actions for web interface](customize/custom-actions.md)

Sympa and other systems
-----------------------

### Integrating Sympa with other systems

  - [LMTP delivery](customize/lmtp-delivery.md)
  - ~~OpenSocial integration~~ (under development)
  - Placing Sympa behind mail relay
  - [Placing WWSympa behind reverse proxy](customize/reverse-proxy.md)
  - Sympa high availability (HA) cluster

### Other topics

  - Migrating from other mailing list systems

