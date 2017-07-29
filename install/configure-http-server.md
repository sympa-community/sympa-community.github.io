Configure HTTP server
=====================

Required Sympa configurations
-----------------------------

Ensure that sympa.conf includes appropriate values for these parameters:
[``wwsympa_url``](../man/sympa.conf.5.md#wwsympa_url),
[``static_content_url``](../man/sympa.conf.5.md#static_content_url)
and [``static_content_path``](../man/sympa.conf.5.md#static_content_path).

* ``wwsympa_url`` is URL prefix of WWSympa service _without_ trailing slash (``/'').
  A common value is ``http://*web.server*/sympa`` or ``https://*web.server*/sympa``.  Servers operated for long use ``http://*web.server*/wws`` or ``https://*web.server*/wws``. Anyway you may use any prefix you prefer: The name of *web.server* may or may not be the same as mail domain.

* ``static_content_url`` is URL path or full path corresponding to ``static_content_path``.  The web server have to be configured mapping the latter to the former.

* Default value of ``static_content_path`` is definied by ``STATICDIR`` in [Sympa/Constants.pm](../man/Sympa-Constants.3.md).  Basically use the default.

Instruction by HTTP servers
---------------------------

- [Apache HTTP Server](configure-http-server-apache.md)
- [lighttpd](configure-http-server-lighttpd.md)
- [nginx](configure-http-server-nginx.md)

Optional configuration
----------------------

- [SOAP HTTP server](configure-http-server-soap.md)

