Configure HTTP server: nginx
============================

Requirements
------------

* [nginx](https://nginx.org/en/download.html).

* [spawn-fcgi](https://redmine.lighttpd.net/projects/spawn-fcgi/wiki), an implementation of FastCGI server.

* [FCGI](https://metacpan.org/release/FCGI), FastCGI interface for Perl.

General instruction
-------------------

1. Register service.

   * If your system supports Systemd, edit ``nginx-wwsympa.service`` in Sympa
     source as you prefer, and copy it to Systemd system directory
     (such as ``/usr/lib/systemd/system``) as ``wwsympa.service`` file.

   * If your system supports system V init script, edit
     [example init script](../examples/initscripts/wwsympa) as you prefer, and
     copy it to sysV init directory (such as ``/etc/rc.d/init.d``) as
     ``wwsympa`` file.

2. Add following excerpt to nginx configuration and edit it as you prefer
   (Note: replace [``$PIDDIR``](../layout.md#piddir),
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

3. Restart nginx.

