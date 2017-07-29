Configure HTTP server: Apache HTTP Server
=========================================

Requirements
------------

* [lighttpd](http://redmine.lighttpd.net/projects/lighttpd/wiki/GetLighttpd).

* [mod_fastcgi](https://redmine.lighttpd.net/projects/1/wiki/docs_modfastcgi) module. On some binary releases, this module may be distributed as separate package.

* [FCGI](https://metacpan.org/release/FCGI), FastCGI interface for Perl.

General instruction
-------------------

1. Edit [example configuration](../examples/ligttpd/sympa.conf) as you prefer,
   and add it to lighttpd configuration.

2. Restart lighttpd.

