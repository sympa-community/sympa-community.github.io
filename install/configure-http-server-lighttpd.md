Configure HTTP server: lighttpd
===============================

Requirements
------------

* [lighttpd](http://redmine.lighttpd.net/projects/lighttpd/wiki/GetLighttpd).

* lighttpd's [mod_fastcgi](https://redmine.lighttpd.net/projects/1/wiki/docs_modfastcgi) module. On several binary releases, this module may be distributed as a separate package.

* [FCGI](https://metacpan.org/release/FCGI), FastCGI interface for Perl.

General instruction
-------------------

1. Edit [example configuration](../examples/lighttpd/sympa.conf) as you prefer,
   and add it to lighttpd configuration.

2. Restart lighttpd.

