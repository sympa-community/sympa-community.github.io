Configure HTTP server
=====================

Requirements
------------

* Mail domain for mailing list service.  It must have been chosen when you
  [configured mail server](configure-mail-server.md).

  In the instructions below, ``mail.example.org`` is used.

Also, you should take care of following parameters:

* Value of [``wwsympa_url``](../man/sympa.conf.5.md#wwsympa_url) parameter.
  This is URL prefix of WWSympa service _without_ trailing slash (``/``).
  A common value is ``http://www.example.org/sympa`` or
  ``https://www.example.org/sympa``.  Servers operated for long years
  sometimes use ``http://www.example.org/wws`` and so on.

  Anyway you may use any prefix you prefer: The host part ``www.example.org``
  may or may not be the same as mail domain; they rather may or may not be the
  subdomains of common domain.

* Value of
  [``static_content_path``](../man/sympa.conf.5.md#static_content_path)
  parameter.  The default is defined by ``STATICDIR`` in Sympa/Constants.pm.
  Basically use default.

* Value of [``static_content_url``](../man/sympa.conf.5.md#static_content_url)
  parameter.  This is URL path or full URL corresponding to
  ``static_content_path``.  Default value is ``/static-sympa``.
  The web server have to be configured so that it wil map the latter to the
  former.

See ["Web interface parameters" in sympa.conf(5)](../man/sympa.conf.5#web-interface-parameters) for more parameters for web interface.

Configure Sympa
---------------

There are two ways to integrate Sympa to Postfix:
_virtual domain_ setting and _single domain_ setting.

The former is recommended.  However, if you will never have plan to manage
multiple domains, the latter is easier way.

### Adding virtual domain

1. If directories for virtual domain configurations were not created,
   create them:
   ```
   # mkdir -m 755 /etc/sympa/mail.example.org
   # touch /etc/sympa/mail.example.org/robot.conf
   # chown -r sympa:sympa /etc/sympa/mail.example.org
   # mkdir -m 750 /var/lib/sympa/list_data/mail.example.org
   # chown sympa:sympa /var/lib/sympa/list_data/mail.example.org
   ```
2. Edit robot.conf created in the step above to add parameters described in
   previous section.

### Single domain

1. Edit /etc/sympa/sympa.conf to add parameters described in previous section.

Configure HTTP server: Instruction by HTTP servers
--------------------------------------------------

WWSympa (web interface of Sympa) itself supports name-based virtual domain.
HTTP server need not handle virtual domains if it is not especially necessary.

- [Apache HTTP Server](configure-http-server-apache.md)
- [lighttpd](configure-http-server-lighttpd.md)
- [nginx](configure-http-server-nginx.md)

Optional configuration
----------------------

- [SOAP HTTP server](configure-http-server-soap.md)

