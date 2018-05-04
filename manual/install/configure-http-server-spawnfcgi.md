---
title: 'Configure HTTP server: Using separate FastCGI service'
prev: configure-mail-server.md
up: configure-http-server.md
next: configure-http-server.md#tests
redirect_from:
  - configure-http-server-nginx.html
---

Configure HTTP server: Using separate FastCGI service
=====================================================

Requirements
------------

  * HTTP server.

    Currently, [nginx](https://nginx.org/en/download.html)
    and Apache HTTP Serever (2.4 or later) are reported working.

    ----
    Note:

      * For Apache HTTP Server:

          * Instruction described here needs mod_proxy_fcgi module introduced
            by HTTP Server 2.4.
            See [another instruction](configure-http-server-apache.md) to know
            about configuration with HTTP Server prior to 2.4.

          * Sympa prior to 6.2.25b.1 has
            [a bug](https://github.com/sympa-community/sympa/pull/164) and
            doesn't work properly with the method described in this chapter.
            Upgrading to recent version is recommended, or see
            [another instruction](configure-http-server-apache.md).

    ----

  * [spawn-fcgi](https://redmine.lighttpd.net/projects/spawn-fcgi/wiki),
    an implementation of FastCGI server.

  * [FCGI](https://metacpan.org/release/FCGI), FastCGI interface for Perl.

General instruction
-------------------

First, install WWSympa FCGI service.  And then configure HTTP server if
necessary.

### Install WWSympa FCGI service

#### Systemd

  1. Register WWSympa FastCGI service.

     Edit [example ``wwsympa.service``](../examples/systemd/wwsympa.service)
     as you prefer, and copy it to Systemd system directory
     (such as ``/usr/lib/systemd/system``) as ``wwsympa.service`` file.

     ----
     Note:

       * If you installed Sympa from source, you may find a file
         ``nginx-wwsympa.service`` in ``src/etc/script`` subdirectory of
         source tree.  Use it as ``wwsympa.service``.

     ----

  2. Start WWSympa FastCGI service.
     ```bash
     # systemctl start wwsympa.service
     # systemctl status wwsympa.service
     ```

  3. Activate WWSympa FastCGI service.
     ```bash
     # systemctl enable wwsympa.service
     ```

#### FreeBSD ports/package

  1. Configure and activate the spawn-fcgi service (Note:
     Replace [``$EXECCGIDIR``](../layout.md#execcgidir) and
     [``$PIDDIR``](../layout.md#piddir) below):
     ``` bash
     # sysrc spawn_fcgi_enable="YES"
     # sysrc spawn_fcgi_app="/usr/local/bin/perl"
     # sysrc spawn_fcgi_bindsocket="$PIDDIR/wwsympa.socket"
     # sysrc spawn_fcgi_bindsocket_mode="0600 -U www"
     # sysrc spawn_fcgi_username="sympa"
     # sysrc spawn_fcgi_groupname"sympa"
     ```

  2. Start WWSympa FastCGI service.
     ```bash
     # /usr/local/etc/rc.d/spawn-fcgi start
     ```

#### System V init script

  1. If your system supports system V init script, edit
     [example init script](../examples/initscripts/wwsympa) as you prefer,
     and copy it to system V init directory (such as ``/etc/rc.d/init.d``)
     as the ``wwsympa`` file.

  2. Start WWSympa FastCGI service.
     ```bash
     # service wwsympa start
     # service wwsympa status
     ```

  3. Activate WWSympa FastCGI service.
     ```bash
     # chkconfig wwsympa on
     ```

### Setup HTTP server

If you have not added configuration for Sympa to HTTP server, follow
instruction below.

#### nginx

  1. Add following excerpt to nginx configuration (Note:
     replace [``$PIDDIR``](../layout.md#piddir),
     [``$EXECCGIDIR``](../layout.md#execcgidir) and
     [``$STATICDIR``](../layout.md#staticdir) below):
     ``` code
     server {
         listen      80;
         server_name localhost.localdomain;  # Change it!

         location /sympa {
             include       /etc/nginx/fastcgi_params;
             fastcgi_pass  unix:$PIDDIR/wwsympa.socket;

             # If you changed wwsympa_url in sympa.conf, change this regex too!
             fastcgi_split_path_info ^(/sympa)(.*)$;
             fastcgi_param SCRIPT_FILENAME $EXECCGIDIR/wwsympa.fcgi;
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

  2. Edit it as you prefer.

     Note that ``server_name`` directive above should contain host part of
     [``wwsympa_url``](../man/sympa.conf.5.md#wwsympa_url) parameter.  Because
     Sympa refers to ``SERVER_NAME`` CGI environment variable to determine
     web host name of the service.

  3. Restart nginx.
     Then test configuration according to
     [instruction](configure-http-server.md#tests).

#### Apache HTTP Server

  1. Add following excerpt to httpd configuration (Note:
     replace [``$PIDDIR``](../layout.md#piddir) and
     [``$STATICDIR``](../layout.md#staticdir) below):
     ``` code
     LoadModule alias_module modules/mod_alias.so
     LoadModule proxy_module modules/mod_proxy.so
     LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so

     ...

     <Location /sympa>
         SetHandler "proxy:unix:$PIDDIR/wwsympa.socket|fcgi://"
         Require all granted
     </Location>

     <Location /static-sympa>
         Require all granted
     </Location>
     Alias /static-sympa $STATICDIR
     ```

  2. Edit it as you prefer.

     Note that ``ServerName`` directive above should contain host part of
     [``wwsympa_url``](../man/sympa.conf.5.md#wwsympa_url) parameter.  Because
     Sympa refers to ``SERVER_NAME`` CGI environment variable to determine
     web host name of the service.

  3. Restart httpd.
     Then test configuration according to
     [instruction](configure-http-server.md#tests).
