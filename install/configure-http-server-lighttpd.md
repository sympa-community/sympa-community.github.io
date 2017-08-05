---
title: 'Configure HTTP server: lighttpd'
prev: configure-mail-server.md
up: configure-http-server.md
next: start-mailing-list-service.md
---

Configure HTTP server: lighttpd
===============================

Requirements
------------

* [lighttpd](http://redmine.lighttpd.net/projects/lighttpd/wiki/GetLighttpd).

* lighttpd's [mod_fastcgi](https://redmine.lighttpd.net/projects/1/wiki/docs_modfastcgi) module. On several binary releases, this module may be distributed as a separate package.

* [FCGI](https://metacpan.org/release/FCGI), FastCGI interface for Perl.

General instruction
-------------------

1. Add following excerpt to lighttpd configuration and edit it as you prefer
   (Note: replace [``$LIBEXECDIR``](../layout.md#libexecdir) and
   [``$STATICDIR``](../layout.md#staticdir)):
   ```
   server.modules += ("mod_fastcgi")

   alias.url += ( "/static-sympa/" => "$STATICDIR/" )

   $HTTP["url"] =~ "\^/sympa" {
   fastcgi.server = ( "/sympa" =>
       ((    "check-local"    =>    "disable",
           "bin-path"    =>    "$LIBEXECDIR/wwsympa-wrapper.fcgi",
           "socket"    =>    "/var/run/lighttpd/sympa.sock",
           "max-procs"    =>     2,
           "idle-timeout"    =>     20,
       ))
   )
   }
   ```

2. Restart lighttpd.

