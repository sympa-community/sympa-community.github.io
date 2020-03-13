---
title: 'Configure HTTP server: Using separate FastCGI service'
prev: configure-mail-server.md
up: configure-http-server.md
next: configure-http-server.md#tests
redirect_from:
  - 'configure-http-server-nginx.html'
---

Configure HTTP server: Running separate FastCGI service
=======================================================

Requirements
------------

  * HTTP server.

    Currently, [nginx](https://nginx.org/en/download.html)
    and [Apache HTTP Server](https://httpd.apache.org/download.cgi)
    (2.4 or later) are reported working.

    ----
    Note:

      * For Apache HTTP Server:
        If you fall under any of following,
        see [another instruction](configure-http-server-apache.md) to know
        about configuration with HTTP Server.

          * You are using HTTP Server prior to version 2.4.
            Instruction described in this chapter needs
            [mod_proxy_fcgi](https://httpd.apache.org/docs/mod/mod_proxy_fcgi.html)
            module introduced by HTTP Server 2.4.

          * Sympa prior to 6.2.25b.1.  It has
            [a bug](https://github.com/sympa-community/sympa/pull/164) and
            doesn't work properly with the method described in this chapter.
            If possible, upgrading to recent version is recommended.

         * Some binary distributions such as:

             - Debian (at least buster or earlier).

             - RPM for RHEL/CentOS 6.

           They have not introduced the method described in this chapter.

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

----
Note:

  * Systemd support with FastCGI services was introduced on Sympa 6.2.15.

----

  1. Register WWSympa FastCGI service.

     Put ``wwsympa.service`` file into Systemd system directory
     (such as ``/usr/lib/systemd/system``).

       * With binary distributions, ``wwsympa.service`` file may have already
         been installed, if that package supports Systemd.
 
         ----
         Note:
         
           * With RPM (RHEL/CentOS 7 or Fedora), this file is prepared by
             `sympa-httpd` package. 

         ----
       * If you have installed Sympa from source, and you have given
         ``--with-unitsdir=DIR`` option to `configure` script,
         you may find a file
         ``wwsympa.service`` in ``src/etc/script`` subdirectory of
         source tree.

         ----
         Note:

           * On Sympa prior to 6.2.36, you may find a file
             ``nginx-wwsympa.service``.  Use it as ``wwsympa.service``.

         ----

     Whichever web server you use, the distributed service file will work.
     The only thing to take care of is the user defined by the -U option
     in FCGI_OPTS:
     ``` code
     FCGI_OPTS="-M 0600 -U apache"
     ```
     The value of this parameter ('``apache``' in the example) must be set
     to the username used by your web server (``www-data`` for
     Apache HTTP Server in Debian, ``nginx`` for nginx on most of
     distributions, etc.)

     You can keep this parameter by adding the line above to a file
     ``/etc/sysconfig/sympa``.

     ----
     Note:

       * You can also serve
         [Sympa SOAP interface](../customize/soap-api.md) with this method.
         Follow the same instructions but with
         ``sympasoap.service`` (or ``nginx-sympasoap.service``) file.

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
     # sysrc spawn_fcgi_app_args="$EXECCGIDIR/wwsympa.fcgi"
     # sysrc spawn_fcgi_bindsocket="$PIDDIR/wwsympa.socket"
     # sysrc spawn_fcgi_bindsocket_mode="0600 -U www"
     # sysrc spawn_fcgi_username="sympa"
     # sysrc spawn_fcgi_groupname"sympa"
     ```

     ----
     Note:

       * If you also want to serve
         [Sympa SOAP interface](../customize/soap-api.md) with this method,
         you may have to write an rc script to start another spawn-fcgi
         service.  Contribution by readers will be appreciated.

     ----

  2. Start WWSympa FastCGI service.
     ```bash
     # /usr/local/etc/rc.d/spawn-fcgi start
     ```

#### System V init script

  1. If your system supports system V init script, edit
     [example init script](../examples/initscripts/wwsympa) as you prefer,
     and copy it to system V init directory (such as ``/etc/rc.d/init.d``)
     as the ``wwsympa`` file.

     ----
     Note:

       * You can also serve Sympa SOAP interface with this method. Follow the
         same instructions but with this
         [example init script](../examples/initscripts/sympasoap). Copy it as
         the ``sympasoap`` file.

     ----
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
     replace [``$PIDDIR``](../layout.md#piddir) and
     [``$STATICDIR``](../layout.md#staticdir) below):
     ``` code
     server {
         listen      80;
         server_name localhost.localdomain;  # Change it!

         location /sympa {
             include       /etc/nginx/fastcgi_params;
             fastcgi_pass  unix:$PIDDIR/wwsympa.socket;
         }

         location /static-sympa {
             alias $STATICDIR;
         }
     }
     ```

     For the SOAP interface, add this in your ``server`` configuration (Note:
     replace [``$PIDDIR``](../layout.md#piddir)):
     ```code
         location /sympasoap {
             include       /etc/nginx/fastcgi_params;
             fastcgi_pass  unix:$PIDDIR/sympasoap.socket;
         }
     ```
     See also a note below.

     ----
     Notes:

       * Some binary distributions ship configuration ready to edit:

           - On RPM, ``/etc/nginx/conf.d/sympa.conf`` file is prepared by
             ``sympa-nginx`` package.

       * With earlier version of Sympa, you may have to add following things:

           - With Sympa 6.2.54 or earlier, insert these into the section of
             "`location /sympa`":
             ``` code
             fastcgi_split_path_info ^(/sympa)(.*)$;
             fastcgi_param PATH_INFO $fastcgi_path_info;
             ```
             and these into the section of "`location /sympasoap`", if any:
             ``` code
             fastcgi_split_path_info ^(/sympasoap)(.*)$;
             fastcgi_param PATH_INFO $fastcgi_path_info;
             ```

           - Additionally, with Sympa 6.2.19b.2 or earlier, insert this
             into the section of "`location /sympa`" (Note:
             replace [``$EXECCGIDIR``](../layout.md#execcgidir)).
             ``` code
             fastcgi_param SCRIPT_FILENAME $EXECCGIDIR/wwsympa.fcgi;
             ```
     ----

  2. Edit it as you prefer.

     Note that ``server_name`` directive above should contain host part of
     [``wwsympa_url``](/gpldoc/man/sympa.conf.5.html#wwsympa_url) parameter.  Because
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

     For SOAP interface, add:

     ```
     <Location /sympasoap>
         SetHandler "proxy:unix:$PIDDIR/sympasoap.socket|fcgi://"
         Require all granted
     </Location>
     ```

     ----
     Note:

       * Some binary distributions ship configuration ready to edit:

           - On RPM (RHEL/CentOS 7 or Fedora), ``/etc/httpd/conf.d/sympa.conf``
             file is prepared by ``sympa-httpd`` package.

     ----
  2. Edit it as you prefer.

     Note that ``ServerName`` directive above should contain host part of
     [``wwsympa_url``](/gpldoc/man/sympa.conf.5.html#wwsympa_url) parameter.  Because
     Sympa refers to ``SERVER_NAME`` CGI environment variable to determine
     web host name of the service.

  3. Restart httpd.
     Then test configuration according to
     [instruction](configure-http-server.md#tests).
