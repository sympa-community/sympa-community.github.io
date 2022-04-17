---
title: 'Configure HTTP server: Setup HTTP server as FastCGI proxy'
up: configure-http-server.md
next: configure-http-server.md#tests
---

Configure HTTP server: Setup HTTP server as FastCGI proxy
=========================================================

  * Instructions in this chepter are applied to the HTTP servers
    accompany with any of these setting methods:

      - [Using Systemd socket](configure-http-server-systemdsocket.md)
      - [Running separate FastCGI service](configure-http-server-spawnfcgi.md)

    They are not applied to servers with earlier (no longer recommended)
    methods.

---

If you have not added configuration for Sympa to HTTP server, follow
instruction below.

Instruction by HTTP servers
---------------------------

### nginx

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

     Additionally with Sympa 6.2.28 or later, it is possible to set
     separate paths for style sheets and pictures.

       - If either or both of parameters
         [`css_path`](/gpldoc/man/sympa_config.5.html#css_path) and
         [`css_url`](/gpldoc/man/sympa_config.5.html#css_url) were changed
         from the default, you also need to add the following settings
         (Note: replace `$css_url` and `$css_path` below):
         ``` code
         location $css_url {
             alias $css_path;
         }
         ```

       - If either or both of parameters
         [`pictures_path`](/gpldoc/man/sympa_config.5.html#pictures_path)
         and
         [`pictures_url`](/gpldoc/man/sympa_config.5.html#pictures_url)
         were changed from the default, you also need to add the following
         settings (Note: replace `$pictures_url` and `$pictures_path`
         below):
         ``` code
         location $pictures_url {
             alias $pictures_path;
         }
         ```

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
     [``wwsympa_url``](/gpldoc/man/sympa_config.5.html#wwsympa_url) parameter.
     Because
     Sympa refers to ``SERVER_NAME`` CGI environment variable to determine
     web host name of the service.

  3. Restart nginx.
     Then test configuration according to
     [instruction](configure-http-server.md#tests).

### Apache HTTP Server

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
	       # Don't forget to edit lines below!
         Require local
         #Require all granted
     </Location>

     <Location /static-sympa>
	       # Don't forget to edit lines below!
         Require local
         #Require all granted
     </Location>
     Alias /static-sympa $STATICDIR
     ```

     For SOAP interface, add:

     ```
     <Location /sympasoap>
         SetHandler "proxy:unix:$PIDDIR/sympasoap.socket|fcgi://"
	       # Don't forget to edit lines below!
         Require local
         #Require all granted
     </Location>
     ```

     Additionally with Sympa 6.2.28 or later, it is possible to set
     separate paths for style sheets and pictures.

       - If either or both of parameters
         [`css_path`](/gpldoc/man/sympa_config.5.html#css_path) and
         [`css_url`](/gpldoc/man/sympa_config.5.html#css_url) were changed
         from the default, you also need to add the following settings
         (Note: replace `$css_url` and `$css_path` below):
         ``` code
         <Location $css_url>
             Require all granted
         </Location>
         Alias $css_url $css_path
         ```

       - If either or both of parameters
         [`pictures_path`](/gpldoc/man/sympa_config.5.html#pictures_path)
         and
         [`pictures_url`](/gpldoc/man/sympa_config.5.html#pictures_url)
         were changed from the default, you also need to add the following
         settings (Note: replace `$pictures_url` and `$pictures_path`
         below):
         ``` code
         <Location $pictures_url>
             Require all granted
         </Location>
         Alias $pictures_url $pictures_path
         ```

     ----
     Note:

       * Some binary distributions ship configuration ready to edit:

           - On RPM (RHEL/CentOS 7 or Fedora), ``/etc/httpd/conf.d/sympa.conf``
             file is prepared by ``sympa-httpd`` package.

     ----
  2. Edit it as you prefer.

     Note that ``ServerName`` directive above should contain host part of
     [``wwsympa_url``](/gpldoc/man/sympa_config.5.html#wwsympa_url) parameter.
     Because
     Sympa refers to ``SERVER_NAME`` CGI environment variable to determine
     web host name of the service.

  3. Restart httpd.
     Then test configuration according to
     [instruction](configure-http-server.md#tests).

### lighttpd

  1. Add following excerpt to lighttpd configuration (Note:
     replace [``$PIDDIR``](../layout.md#piddir) and
     [``$STATICDIR``](../layout.md#staticdir)):
     ```
     server.modules += ("mod_fastcgi")
     server.modules += ("mod_alias")

     alias.url += ( "/static-sympa/" => "$STATICDIR/" )

     $HTTP["url"] =~ "^/sympa" {
     fastcgi.server = ( "/sympa" =>
         ((  "check-local" => "disable",
             "socket"      => "$PIDDIR/wwsympa.socket",
         ))
     )
     }
     ```

     Additionally with Sympa 6.2.28 or later, it is possible to set
     separate paths for style sheets and pictures.

       - If either or both of parameters
         [`css_path`](/gpldoc/man/sympa_config.5.html#css_path) and
         [`css_url`](/gpldoc/man/sympa_config.5.html#css_url) were changed
         from the default, you also need to add the following settings
         (Note: replace `$css_url` and `$css_path` below):
         ``` code
         alias.url += ( "$css_url/" => "$css_path/" )
         ```

       - If either or both of parameters
         [`pictures_path`](/gpldoc/man/sympa_config.5.html#pictures_path)
         and
         [`pictures_url`](/gpldoc/man/sympa_config.5.html#pictures_url)
         were changed from the default, you also need to add the following
         settings (Note: replace `$pictures_url` and `$pictures_path`
         below):
         ``` code
         alias.url += ( "$pictures_url/" => "$pictures_path/" )
         ```

  2. Edit it as you prefer.

  3. Restart lighttpd.
     Then test configuration according to
     [instruction](configure-http-server.md#tests).

