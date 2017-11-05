---
title: 'Configure HTTP server: nginx'
prev: configure-mail-server.md
up: configure-http-server.md
next: configure-http-server.md#tests
---

Configure HTTP server: nginx
============================

Requirements
------------

  * [nginx](https://nginx.org/en/download.html).

  * [spawn-fcgi](https://redmine.lighttpd.net/projects/spawn-fcgi/wiki),
    an implementation of FastCGI server.

  * [FCGI](https://metacpan.org/release/FCGI), FastCGI interface for Perl.

General instruction
-------------------

  1. Register WWSympa FastCGI service.

       * Systemd

         Edit [example ``wwsympa.service``](../examples/systemd/wwsympa.service)
         as you prefer, and copy it to Systemd system directory
         (such as ``/usr/lib/systemd/system``) as ``wwsympa.service`` file.

         ----
         Note:

           * If you installed Sympa from source, you may find a file
             ``nginx-wwsympa.service`` in ``src/etc/script`` subdirectory of
             source tree.  Use it as ``wwsympa.service``.

         ----

       * System V init script

         If your system supports system V init script, edit
         [example init script](../examples/initscripts/wwsympa) as you prefer,
         and copy it to system V init directory (such as ``/etc/rc.d/init.d``)
         as the ``wwsympa`` file.

  2. Start WWSympa FastCGI service.

       * Systemd
         ```bash
         # systemctl start wwsympa.service
         # systemctl status wwsympa.service
         ```

       * initscripts or FreeBSD ports
         ```bash
         # service wwsympa start
         # service wwsympa status
         ```

  3. Activate WWSympa FastCGI service.

       * Systemd
         ```bash
         # systemctl enable wwsympa.service
         ```

       * initscripts
         ```bash
         # chkconfig wwsympa on
         ```

  4. If you have not added configuration for Sympa to nginx, add following
     excerpt (Note: replace [``$PIDDIR``](../layout.md#piddir),
     [``$LIBEXECDIR``](../layout.md#libexecdir) and
     [``$STATICDIR``](../layout.md#staticdir) below):
     ```
     server {
         listen      80;
         server_name localhost.localdomain;  # Change it!

         location /sympa {
             include       /etc/nginx/fastcgi_params;
             fastcgi_pass  unix:$PIDDIR/wwsympa.socket;

             # If you changed wwsympa_url in sympa.conf, change this regex too!
             fastcgi_split_path_info ^(/sympa)(.*)$;
             fastcgi_param SCRIPT_FILENAME $LIBEXECDIR/wwsympa.fcgi;
             fastcgi_param PATH_INFO $fastcgi_path_info;
         }

         location /static-sympa {
             alias $STATICDIR;
         }
     }
     ```

     ----
     Note:

       * Some binary distributions ship configuration ready to edit:

           - On RPM, ``/etc/nginx/conf.d/sympa.conf`` file is prepared by
             ``sympa-nginx`` package.

     ----

  5. Edit it as you prefer.

     Note that ``server_name`` directive above should contain host part of
     [``wwsympa_url``](../man/sympa.conf.5.md#wwsympa_url) parameter.  Because
     Sympa refers to ``SERVER_NAME`` CGI environment variable to determine
     web host name of the service.

  6. Restart nginx.

