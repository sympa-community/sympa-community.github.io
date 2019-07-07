---
title: 'Requirements'
prev: scope.md
up: ./
next: install.md
---

Requirements
============

Software requirements
---------------------

### Operating system

Sympa supports POSIX-based Unix-like operating systems.  At least following
operating systems have been reported working:

  - Several GNU/Linux distributions: Debian / Ubuntu / Linux Mint,
    RHEL / CentOS / Fedora, Slackware, etc.
  - FreeBSD (2.x or later), NetBSD, OpenBSD.
  - macOS.
  - Tru64 UNIX.
  - Solaris (2.5 or later).
  - AIX (V4 or later).
  - HP-UX (10.20).

Sympa does not support Microsoft Windows.

### Perl interpreter

[Perl 5 interpreter](https://www.perl.org/get.html) is required.
Currently, Perl 5.10.1 and later are supported, however, recent version of
Perl 5 is recommended.

----
Note:

  * As of Sympa 6.2.45b, support for Perl 5.10.0 or earlier has been dropped.

----

### RDBMS

SQL server should have been installed and can be managed.  At least following
systems are supported:

  - [MySQL](https://dev.mysql.com/downloads/) (4.1.1 or later)
    or [MariaDB](https://mariadb.com/downloads/mariadb-tx).
  - [PostgreSQL](https://www.postgresql.org/download/) (7.4 or later).
  - Oracle Database (9i or later is recommended).
  - [SQLite](https://www.sqlite.org/download.html) (3.x).

Though Sympa can share database server with the other systems, dedicated
database server is strongly recommended.

SQLite does not need actual server: It directly accesses database file on
local storage.  Using it on the sites with low volume is worth consideration.

### Mail transfer agent (MTA)

Mail server is required.  At least following products are reported working
with Sympa:

  - [Sendmail](https://www.proofpoint.com/us/sendmail-open-source) (V8).
  - [Postfix](http://www.postfix.org/) (2.x or 3.x).
  - [exim](http://www.exim.org/mirrors.html).
  - [OpenSMTPD](https://www.opensmtpd.org/).
  - qmail.  Use of this is discouraged because it lacks modern features.

### HTTP server

To provide web interface, HTTP server is required.  At least following
products are reported working with Sympa:

  - [Apache HTTP Server](http://httpd.apache.org/download.cgi).
  - [lighttpd](http://redmine.lighttpd.net/projects/lighttpd/wiki/GetLighttpd).
  - [nginx](https://nginx.org/en/download.html).

### Other software components

Sympa depends on several software components including tens of external Perl
modules.  Mandatory modules will be installed during
[installation process](install.md).  More modules required to enable optional
features will be described in each section of this document.

Network requirements
--------------------

  * Inbound and outbound SMTP connections (typically on TCP port 25) should be
    allowed.

  * For web interface, inbound HTTP or HTTPS connections (typically on TCP
    port 80 or 443) should be allowed.

  * Any other unnecessary inbound accesses including database connections
    should be inhibited by taking measures such as firewall.

  * Finally, at least one **mail domain name** for the mailing list service is
    indispensable.  Appropriate DNS resource record (``MX``, ``A``, ``AAAA``
    or any of them) for that domain must be advertised.

