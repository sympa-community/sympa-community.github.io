Configure HTTP server: Apache HTTP Server
=========================================

Requirements
------------

* [nginx](https://nginx.org/en/download.html).

* [spawn-fcgi](https://redmine.lighttpd.net/projects/spawn-fcgi/wiki), an implimentation of FastCGI server.

* [FCGI](https://metacpan.org/release/FCGI), FastCGI interface for Perl.

General instruction
-------------------

### Systemd

1. Edit ``nginx-wwsympa.service`` in Sympa source as you prefer, and copy it to Systemd system directory as ``wwsympa.service``.

2. Edit [example configuration](../examples/nginx/sympa.conf) as you prefer,
   and add it to nginx configuration.

3. Restart nginx.

### initscripts

1. Edit [example init script](../examples/initscripts/wwsympa) as you prefer, and copy it to sysV init directory as ``wwsympa``.

2. Edit [example configuration](../examples/nginx/sympa.conf) as you prefer,
   and add it to nginx configuration.

3. Restart nginx.

