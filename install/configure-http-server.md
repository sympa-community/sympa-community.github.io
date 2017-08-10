---
title: 'Configure HTTP server'
prev: configure-mail-server.md
up: ../install.md
next: start-mailing-list-service.md
---

Configure HTTP server
=====================

Requirements
------------

  * HTTP server to provide web interface:
    See also "[Requirements](../requirements.md#http-server)".

  * A mail domain name for the mailing list service.  It must have been chosen
    when you [configured mail server](configure-mail-server.md).

    Through the instructions in this chapter, ``mail.example.org`` will be
    used for example.

  * The URL prefix dedicated for WWSympa service.  Either ``http`` or
    ``https`` scheme may be used.
    The host part may or may not be the same as mail domain name.

    Through the instructions in this chapter, ``http://web.example.org/sympa``
    will be used for example.

  * Several binary distributions need additional packages installed to enable
    web interface.

      - RPM: Install ``sympa-httpd``, ``sympa-lighttpd`` or ``sympa-nginx``
        package according to HTTP servers you will configure.

Sympa configuration parameters
------------------------------

  * [``wwsympa_url``](../man/sympa.conf.5.md#wwsympa_url)

    This is URL prefix of WWSympa service _without_ trailing slash (``/``).

  * [``static_content_url``](../man/sympa.conf.5.md#static_content_url)

    This is URL path or full URL of static content.  Default value is
    ``/static-sympa``.  HTTP server have to map it with
    [``$STATICDIR``](../layout.md#staticdir).

See ["Web interface parameters" in sympa.conf(5)](../man/sympa.conf.5#web-interface-parameters) for more parameters for web interface.

----
Note:

  * Value of [``use_fast_cgi``](../man/sympa.conf.5.md#use_fast_cgi) parameter
    in [``sympa.conf``](../man/sympa.conf.5.md#config) must be ``1``,
    the default.

----

Two ways to integrate
---------------------

There are two ways to integrate Sympa into HTTP server:
_virtual domain_ setting and _single domain_ setting.

The former is recommended.  However, if you will never have plan to manage
multiple domains, the latter is easier way.

Virtual domain setting
----------------------

### Initial setting

Steps in this section may be done once at the first time.

  1. Setup HTTP server according to description in
     "[Instruction by HTTP servers](#instruction-by-http-servers)".

### Adding new domain

  1. If directories for virtual domain configurations have not been created,
     create them (Note: replace [``$SYSCONFDIR``](../layout.md#sysconfdir),
     [``$EXPLDIR``](../layout.md#expldir) and ``mail.example.org`` below):
     ```
     # mkdir -m 755 $SYSCONFDIR/mail.example.org
     # touch $SYSCONFDIR/mail.example.org/robot.conf
     # chown -r sympa:sympa $SYSCONFDIR/mail.example.org
     # mkdir -m 750 $EXPLDIR/mail.example.org
     # chown sympa:sympa $EXPLDIR/mail.example.org
     ```

  2. Edit ``robot.conf`` created by the step above to add parameters described
     in previous section:
     ```
     wwsympa_url http://web.example.org/sympa
     ```

     ----
     Note:

       * On Sympa 6.2.18 or earlier, ``robot.conf`` had to contain additional
         [``http_host``](../man/sympa.conf.5.md#http_host) parameter, like:
         ```
         wwsympa_url http://web.example.org/sympa
         http_host web.example.org/sympa
         ```
         On Sympa 6.2.19b.1 or later, this parameter will not be necessary.
         ``http_host`` parameter may be obsoleted in the future.

     ----

If you want to add another domain, repeat steps in this section by each domain.

Single domain setting
---------------------

  1. Edit [``sympa.conf``](../layout.md#config) to add parameters described in
     previous section:
     ```
     domain (...existing parameter value...)
     listmaster (...existing parameter value...)
     wwsympa_url http://web.example.org/sympa
     ```

  2. Setup HTTP server according to description in
     "[Instruction by HTTP servers](#instruction-by-http-servers)" section.

Instruction by HTTP servers
---------------------------

  - [Apache HTTP Server](configure-http-server-apache.md)
  - [lighttpd](configure-http-server-lighttpd.md)
  - [nginx](configure-http-server-nginx.md)

Tests
-----

  1. Start web browser on your PC or PDA.

  2. Open the URL ``http://web.example.org/sympa`` (the URL you have
     configured).  And confirm that home page of Sympa web interface will be
     shown.

If something went unexpected, check following information:

  - Logs of HTTP server (error log and access log).
  - Sympa log file (see also
    "[Configure system log](configure-system-log.md)").
  - Configuration of HTTP server and Sympa.

