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

  * [Mail addresses](customize/basics-addresses.md)
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

    &#x2699; nrcpt_by_domain.conf

  * Privileges
    [&#x1F527;](man/sympa.conf.5.md#privileges)

    &#x2699; edit_list.conf

  * Archives
    [&#x1F527;](man/sympa.conf.5.md#archives)

    &#x2699; mhonarc_ressources.tt2

  * [Bounce management](customize/bounce-management.md)

  * Loop prevention
    [&#x1F527;](man/sympa.conf.5.md#loop-prevention)

  * [Automatic list creation](customize/automatic-lists.md)

  * Tag based spam filtering
    [&#x1F527;](man/sympa.conf.5.md#tag-based-spam-filtering)

  * Directories
    [&#x1F527;](man/sympa.conf.5.md#directories)

  * Miscelaneous
    [&#x1F527;](man/sympa.conf.5.md#miscelaneous)

  * Others

      - Custom user attributes in database
        [&#x1F527;](man/sympa.conf.5.md#db_additional_subscriber_fields)

Customizing web interface
--------------------------

To manage these features, listmaster privileges and/or console login are
needed.

  * Appearances
    [&#x1F527;](man/sympa.conf.5.md#web-interface-parameters-appearances)

  - Authentication on web interface

    &#x2699; auth.conf

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

        &#x2699; crawlers_detection.conf

  * [Message tracking](customize/message-tracking.md)

  * [RSS feed](customize/rss-feed.md)

  * [User-friendly automatic lists](customize/friendly-automatic-lists.md)

Sympa services: Optional features
---------------------------------

These features need installation of additional software components including
external Perl modules.

  * S/MIME
    [&#x1F527;](man/sympa.conf.5.md#s-mime-and-tls)

    &#x1F4E6;
    [Crypt-OpenSSL-X509](https://metacpan.org/release/Crypt-OpenSSL-X509),
    [Crypt-SMIME](https://metacpan.org/release/Crypt-SMIME).

  * Data sources setup
    [&#x1F527;](man/sympa.conf.5.md#data-sources-setup)

    &#x2699; data_sources/*.incl

    &#x1F4E6; [DBD-CSV](https://metacpan.org/release/DBD-CSV),
    [DBD-mysql](https://metacpan.org/release/DBD-mysql),
    [DBD-ODBC](https://metacpan.org/release/DBD-ODBC),
    [DBD-Oracle](https://metacpan.org/release/DBD-Oracle),
    [DBD-Pg](https://metacpan.org/release/DBD-Pg),
    [DBD-SQLite](https://metacpan.org/release/DBD-SQLite),
    [DBD-Sybase](https://metacpan.org/release/DBD-Sybase) and/or
    [Net-LDAP](https://metacpan.org/release/Net-LDAP).
    Optionally [IO-Socket-SSL](https://metacpan.org/release/IO-Socket-SSL).

  * [DKIM](customize/dkim.md)

  * [DMARC protection](customize/dmarc-protection.md)

  * Managing aliases with LDAP

    &#x2699; ldap_alias_manager.conf
    &#x2699; ldap_alias_entry.tt2

    &#x1F4E6; [Net-LDAP](https://metacpan.org/release/Net-LDAP).
    Optionally [IO-Socket-SSL](https://metacpan.org/release/IO-Socket-SSL).

  * ~~Managing aliases with RDBMS~~ (currently broken)

  * List address verification
    [&#x1F527;](man/sympa.conf.5.md#list-address-verification)

    &#x1F4E6; [libnet](https://metacpan.org/release/libnet)
    ([Net::SMTP](https://metacpan.org/pod/Net::SMTP)).

  * [Antivirus plug-in](customize/antivirus.md)

  * Miscelaneous

    &#x1F4E6; Optionally [Clone](https://metacpan.org/release/Clone),
    [Encode-Locale](https://metacpan.org/release/Encode-Locale) etc.

Web interface: Optional features
--------------------------------

These features need installation of additional software components including
external Perl modules.

  * CAS single sign-on

    &#x2699; auth.conf

    &#x1F4E6; [AuthCAS](https://metacpan.org/release/AuthCAS).

  * TLS client authentication
    [&#x1F527;](man/sympa.conf.5.md#s-mime-and-tls)

    &#x1F4E6;
    [Crypt-OpenSSL-X509](https://metacpan.org/release/Crypt-OpenSSL-X509).

  * Password validation
    [&#x1F527;](man/sympa.conf.5.md#password-validation)

    &#x1F4E6; [Data-Password](https://metacpan.org/release/Data-Password).

  * Authentication with LDAP
    [&#x1F527;](man/sympa.conf.5.md#authentication-with-ldap)

    &#x1F4E6; [Net-LDAP](https://metacpan.org/release/Net-LDAP).
    Optionally [IO-Socket-SSL](https://metacpan.org/release/IO-Socket-SSL).

  * [SOAP/HTTP API](customize/soap-api.md)

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

  - Custom actions for web interface

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

