---
title: 'Configure HTTP server: Apache HTTP Server (compatible with earlier version)'
prev: configure-mail-server.md
up: configure-http-server.md
next: configure-http-server.md#tests
---

Configure HTTP server: Apache HTTP Server (compatible with earlier version)
===========================================================================

----
Note:

  * This chapter describes configuration compatible with earlier version of
    Apache HTTP Server (prior to 2.4).  If you are using version 2.4 or later
    of Apache HTTP Server, see recommended instruction
    [using separate FastCGI service](configure-http-server-spawnfcgi.md).

----

Requirements
------------

  * [Apache HTTP Server](http://httpd.apache.org/download.cgi).

  * [mod_fcgid](http://httpd.apache.org/mod_fcgid/), FastCGI server for
    Apache.
    Note that "mod_fastcgi" is no longer maintained and highly discouraged.

  * [FCGI](https://metacpan.org/release/FCGI), FastCGI interface for Perl.

----
Note:

  * [`wwsympa.fcgi`](/gpldoc/man/wwsympa.8.html) is wrapped in small setuid program
    written in C, [`wwsympa-wrapper.fcgi`](/gpldoc/man/wwsympa-wrapper.8.html).

    Setuid wrapper was introduced on Sympa 5.4
    in order to avoid to use the --- insecure and no longer
    maintained --- setuid perl mode.

    With HTTP Server 2.4 or later, another installation method
    [using separate FastCGI service](configure-http-server-spawnfcgi.md)
    does not need setuid wrapper.

----

General instruction
-------------------

  1. If you have not added configuration for Sympa to httpd, add following
     excerpt (Note: replace [``$EXECCGIDIR``](../layout.md#execcgidir) and
     [``$STATICDIR``](../layout.md#staticdir)):

     For HTTP Server 2.4:
     ```
     <Location /sympa>
         SetHandler fcgid-script

         # Don't forget to edit lines below!
         Require all denied
         #Require all granted
     </Location>
     ScriptAlias /sympa $EXECCGIDIR/wwsympa-wrapper.fcgi

     ## You may uncomment following lines to enable SympaSOAP feature.
     #<Location /sympasoap>
     #    SetHandler fcgid-script
     #
     #    # Don't forget to edit lines below!
     #    Require all denied
     #    #Require all granted
     #</Location>
     #ScriptAlias /sympasoap $EXECCGIDIR/sympa_soap_server-wrapper.fcgi

     # Other static contents
     Alias /static-sympa $STATICDIR

     ## If your host is dedicated to Sympa:
     #RewriteEngine on
     #RewriteRule ^/?$ /sympa [R=301]
     ```

     For HTTP Server 2.2:
     ```
     <Location /sympa>
         SetHandler fcgid-script

         # Don't forget to edit lines below!
         Order deny,allow
         Deny from all
         #Allow from all
     </Location>
     ScriptAlias /sympa $EXECCGIDIR/wwsympa-wrapper.fcgi

     ## You may uncomment following lines to enable SympaSOAP feature.
     #<Location /sympasoap>
     #    SetHandler fcgid-script
     #
     #    # Don't forget to edit lines below!
     #    Order deny,allow
     #    Deny from all
     #    #Allow from all
     #</Location>
     #ScriptAlias /sympasoap $EXECCGIDIR/sympa_soap_server-wrapper.fcgi

     # Other static contents
     Alias /static-sympa $STATICDIR

     ## If your host is dedicated to Sympa:
     #RewriteEngine on
     #RewriteRule ^/?$ /sympa [R=301]
     ```

     ----
     Note:

       * Some binary distributions ship configuration ready to edit:

           - On Debian (10 "buster" or earlier),
             ``/etc/apache2/conf-available/sympa.conf`` file is prepared.

           - On RPM (RHEL/CentOS 6), ``/etc/httpd/conf.d/sympa.conf`` file is
             prepared by ``sympa-httpd`` package.

     ----

  2. Edit it as you prefer.

     Note that ``ServerName`` or ``ServerAlias`` directive should define
     the host part of [``wwsympa_url``](/gpldoc/man/sympa.conf.5.html#wwsympa_url)
     parameter.  Because Sympa refers to ``SERVER_NAME`` CGI environment
     variable to determine host name of web service.

     You may also tune FastCGI by adding directives such as
     ``FcgidIOTimeout``, ``FcgidMaxProcesses`` or ``FcgidMaxRequestLen``.  For
     more details see the
     [mod_fcgid reference page](https://httpd.apache.org/mod_fcgid/mod/mod_fcgid.html).

  3. Restart httpd.

