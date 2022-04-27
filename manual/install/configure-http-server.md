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

  * [MHonArc](https://www.mhonarc.org/) to provide message archives
    browseable by web interface.

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

  * [``wwsympa_url``](/gpldoc/man/sympa_config.5.html#wwsympa_url)

    This is URL prefix of WWSympa service _without_ trailing slash (``/``).

  * [``mhonarc``](/gpldoc/man/sympa_config.5.html#mhonarc)

    This is full path to executable file of MHonArc used to provide archives
    browseable by web interface.

See ["Web interface parameters" in sympa.conf(5)](/gpldoc/man/sympa_config.5.html#web-interface-parameters) for more parameters for web interface.

And following parameter in [``sympa.conf``](../layout.md#config) may be
useful:

  * [``log_facility``](/gpldoc/man/sympa_config.5.html#log_facility)

    Setting this, you can record logs about web interface into separate log
    file.  Default value is the same as
    [``syslog``](/gpldoc/man/sympa_config.5.html#syslog) parameter.

----
Note:

  * On Sympa 6.2.22 or earlier,
    value of [``use_fast_cgi``](/gpldoc/man/sympa_config.5.html#use_fast_cgi) parameter
    in [``sympa.conf``](/gpldoc/man/sympa_config.5.html#config) must be ``1``,
    the default.

----

Two ways to integrate
---------------------

There are two ways to integrate Sympa into HTTP server:
  - [_virtual domain_ setting](#virtual-domain-setting) (managing one _or_ more mail domains)
  - [_single domain_ setting](#single-domain-setting) (managing only one mail domain)

The former is recommended.  However, if you will never have plan to manage
multiple domains, the latter is easier way.

You can not mix both ways. Following sections describe these two ways by each.

Virtual domain setting
----------------------

  1. If path of MHonArc executable file is differ from the default of
     [``mhonarc``](/gpldoc/man/sympa_config.5.html#mhonarc) parameter,
     ``/usr/bin/mhonarc``, define it in
     [``sympa.conf``](../layout.md#config).  For example:

     ```
     mhonarc /usr/local/bin/mhonarc
     ```

  2. If directories for virtual domain configurations have not been created,
     create them (Note: replace [``$SYSCONFDIR``](../layout.md#sysconfdir),
     [``$EXPLDIR``](../layout.md#expldir) and ``mail.example.org`` below):
     ```
     # mkdir -m 755 $SYSCONFDIR/mail.example.org
     # touch $SYSCONFDIR/mail.example.org/robot.conf
     # chown -r sympa:sympa $SYSCONFDIR/mail.example.org
     # mkdir -m 750 $EXPLDIR/mail.example.org
     # chown sympa:sympa $EXPLDIR/mail.example.org
     ```

  3. Edit ``robot.conf`` created by the step above to add parameter(s)
     described in previous section:
     ```
     wwsympa_url http://web.example.org/sympa
     ```

     ----
     Note:

       * On Sympa 6.2.18 or earlier, ``robot.conf`` had to contain additional
         [``http_host``](/gpldoc/man/sympa_config.5.html#http_host) parameter, like:
         ```
         wwsympa_url http://web.example.org/sympa
         http_host web.example.org/sympa
         ```
         There is no reason to use ``http_host`` on later releases.

     ----

  4. Continue setting according to description in
     "[Instruction by HTTP servers](#instruction-by-http-servers)".

If you want to add another domain, repeat steps in this section by each domain.

Single domain setting
---------------------

  1. Edit [``sympa.conf``](../layout.md#config) to add parameter(s) described
     in previous section:
     ```
     domain (...existing parameter value...)
     listmaster (...existing parameter value...)
     wwsympa_url http://web.example.org/sympa
     mhonarc /usr/local/bin/mhonarc     (If path is differ from the default)
     ```

  2. Continue setting according to description in
     "[Instruction by HTTP servers](#instruction-by-http-servers)" section.

Instruction by HTTP servers
---------------------------

These methods are reported to be applicable to Apache HTTP Server
(2.4 or later), nginx and lighttpd.

  - **Linux environments with Systemd support**:
    See "[Using Systemd socket](configure-http-server-systemdsocket.md)".

  - **Other environments**:
    See "[Using separate FastCGI service](configure-http-server-spawnfcgi.md)".

### Obsoleted methods

These pages describe the method using setuid wrappers (`wwsympa-wrapper.fcgi`
and `sympa_soap_server-wrapper.fcgi`) which are no longer recommended.

  - [Apache HTTP Server](configure-http-server-apache.md) (HTTP Server 2.2.x or
    earlier needs this method)
  - [lighttpd](configure-http-server-lighttpd.md)

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

