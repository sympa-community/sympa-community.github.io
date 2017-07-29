Configure HTTP server: Apache HTTP Server
=========================================

Requirements
------------

* [Apache HTTP Server](http://httpd.apache.org/download.cgi).

* [mod_fcgid](http://httpd.apache.org/mod_fcgid/), FastCGI server for Apache.
  Note that "mod_fastcgi" is no longer maintained and highly discouraged.

* [FCGI](https://metacpan.org/release/FCGI), FastCGI interface for Perl.

General instruction
-------------------

1. Edit [example configuration](../examples/httpd/sympa.conf) as you prefer,
   and add it to httpd configuration.

2. Restart httpd.

