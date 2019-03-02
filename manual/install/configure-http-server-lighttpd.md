---
title: 'Configure HTTP server: lighttpd'
prev: configure-mail-server.md
up: configure-http-server.md
next: configure-http-server.md#tests
---

Configure HTTP server: lighttpd
===============================

Requirements
------------

  * [lighttpd](http://redmine.lighttpd.net/projects/lighttpd/wiki/GetLighttpd).

  * lighttpd's
    [mod_fastcgi](https://redmine.lighttpd.net/projects/1/wiki/docs_modfastcgi)
    module. On several binary releases, this module may be distributed as a
    separate package.

  * [FCGI](https://metacpan.org/release/FCGI), FastCGI interface for Perl.

----
Note:

  * [`wwsympa.fcgi`](../man/wwsympa.8.md) is wrapped in small setuid program
    written in C, [`wwsympa-wrapper.fcgi`](../man/wwsympa-wrapper.8.md).

    Setuid wrapper was introduced on Sympa 5.4
    in order to avoid to use the --- insecure and no longer
    maintained --- setuid perl mode.

----

General instruction
-------------------

  1. If you have not added configuration for Sympa to lighttpd, add following
     excerpt (Note: replace [``$EXECCGIDIR``](../layout.md#execcgidir) and
     [``$STATICDIR``](../layout.md#staticdir)):
     ```
     server.modules += ("mod_fastcgi")

     alias.url += ( "/static-sympa/" => "$STATICDIR/" )

     $HTTP["url"] =~ "^/sympa" {
     fastcgi.server = ( "/sympa" =>
         ((    "check-local"    =>    "disable",
             "bin-path"    =>    "$EXECCGIDIR/wwsympa-wrapper.fcgi",
             "socket"    =>    "/var/run/lighttpd/sympa.sock",
             "max-procs"    =>     2,
             "idle-timeout"    =>     20,
         ))
     )
     }
     ```

     ----
     Note:

       * Some binary distributions ship configuration ready to edit:

           - On RPM, ``/etc/lighttpd/sympa.conf`` file is prepared by
             ``sympa-lighttpd`` package.  To add it
             to configuration, you might want to add a line at the bottom in
             ``lighttpd.conf``:
             ```
             include /etc/lighttpd/sympa.conf
             ```

     ----

  2. Edit it as you prefer.

  3. Restart lighttpd.

