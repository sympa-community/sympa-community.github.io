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

> **Note**
>
>   * [`wwsympa.fcgi`](/gpldoc/man/wwsympa.8.html) is wrapped in small setuid program
>     written in C, [`wwsympa-wrapper.fcgi`](/gpldoc/man/wwsympa-wrapper.8.html).
>
>     Setuid wrapper was introduced on Sympa 5.4
>     in order to avoid to use the --- insecure and no longer
>     maintained --- setuid perl mode.

General instruction
-------------------

  1. If you have not added configuration for Sympa to lighttpd, add following
     excerpt (Note: replace [``$EXECCGIDIR``](../layout.md#execcgidir),
     [``$CSSDIR``](../layout.md#cssdir),
     [``$PICTURESDIR``](../layout.md#picturesdir) and
     [``$STATICDIR``](../layout.md#staticdir)):
     ```
     server.modules += ("mod_fastcgi")
     server.modules += ("mod_alias")

     # Line below is needed for 6.2.28 or later.
     alias.url += ( "/static-sympa/css/" => "$CSSDIR/" )
     # Line below is needed for 6.2.28 or later.
     alias.url += ( "/static-sympa/pictures/" => "$PICTURESDIR/" )
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
     In above, `/static-sympa/css`, `/static-sympa/pictures` and
     `/static-sympa` are the default values of
     [`css_url`](/gpldoc/man/sympa_config.5.html#css_url),
     [`pictures_url`](/gpldoc/man/sympa_config.5.html#pictures_url) and
     [`statoc_content_url`](/gpldoc/man/sympa_config.5.html#static_content_url),
     respectively.

     > **Note**
     >
     >   * Some binary distributions ship configuration ready to edit:
     >
     >       - On RPM, ``/etc/lighttpd/conf.d/sympa.conf`` file is prepared by
     >         ``sympa-lighttpd`` package.  To add it
     >         to configuration, you might want to add a line at the bottom in
     >         ``lighttpd.conf``:
     >         ```
     >         include conf.d/sympa.conf
     >         ```

  2. Edit it as you prefer.

  3. Create a directory `/var/run/lighttpd` that is writable by lighttpd processes:
  
     ``` bash
     # mkdir /var/run/lighttpd
     # chown lighttpd /var/run/lighttpd
     ```

  4. Restart lighttpd.

