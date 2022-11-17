---
title: 'Configure HTTP server: Using separate FastCGI service'
prev: configure-mail-server.md
up: configure-http-server.md
next: configure-http-server-proxy.md
redirect_from:
  - 'configure-http-server-nginx.html'
---

Configure HTTP server: Running separate FastCGI service
=======================================================

Requirements
------------

  * HTTP server.

    Currently, [nginx](https://nginx.org/en/download.html),
    [Apache HTTP Server](https://httpd.apache.org/download.cgi)
    (2.4 or later) and [lighttpd](https://www.lighttpd.net/)
    are reported working.

    > **Note**
    >
    >   * For Apache HTTP Server:
    >     If you fall under any of following,
    >     see [another instruction](configure-http-server-apache.md) to know
    >     about configuration with HTTP Server.
    >
    >       * You are using HTTP Server prior to version 2.4.
    >         Instruction described in this chapter needs
    >         [mod_proxy_fcgi](https://httpd.apache.org/docs/mod/mod_proxy_fcgi.html)
    >         module introduced by HTTP Server 2.4.
    >
    >       * Sympa prior to 6.2.25b.1.  It has
    >         [a bug](https://github.com/sympa-community/sympa/pull/164) and
    >         doesn't work properly with the method described in this chapter.
    >         If possible, upgrading to recent version is recommended.

  * [spawn-fcgi](https://redmine.lighttpd.net/projects/spawn-fcgi/wiki),
    an implementation of FastCGI server.

  * [FCGI](https://metacpan.org/release/FCGI), FastCGI interface for Perl.

General instruction
-------------------

First, install WWSympa FCGI service.  And then configure HTTP server if
necessary.

### Install WWSympa FCGI service

#### Systemd

> **Note**
>
>   * Do not mix the instructions in this section with
>     ones in the chapter
>     "[Using Systemd socket](configure-http-server-systemdsocket.md)".
>     They are incompatible each other, and even though they have the same
>     name in the configuration files, their contents are different.
>
>   * Systemd support with FastCGI services was introduced on Sympa 6.2.15.

  1. Register WWSympa FastCGI service.

     Put ``wwsympa.service`` file into Systemd system directory
     (such as ``/usr/lib/systemd/system``).

       * With binary distributions, ``wwsympa.service`` file may have already
         been installed, if that package supports Systemd.
 
         > **Note**
         >
         >   * Some binary distributions ship configuration ready to edit:
         > 
         >       * With RPM (RHEL/CentOS 7) this file is prepared by
         >         `sympa-httpd` or `sympa-nginx` package.

       * If you have installed Sympa from source, and you have given
         ``--with-unitsdir=DIR`` option to `configure` script,
         you may find a file
         ``wwsympa-spawn-fcgi.service`` in ``service`` subdirectory of
         source tree.  Use it as ``wwsympa.service`` (however also check
         the notes below).

         > **Note**
         >
         >   * On Sympa prior to 6.2.70, you may find a file
         >     ``wwsympa.service`` in
         >     ``src/etc/script`` subdirectory of source tree.
         >
         >   * Additinally, on Sympa prior to 6.2.36, you may find a file
         >     ``nginx-wwsympa.service``.  Use it as ``wwsympa.service``.

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

     > **Note**
     >
     >   * You can also serve
     >     [Sympa SOAP interface](../customize/soap-api.md) with this method.
     >     Follow the same instructions but with
     >     ``sympasoap-spawn-fcgi.service`` (or ``sympasoap.service``,
     >     ``nginx-sympasoap.service``) file.

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

     > **Note**
     >
     >   * If you also want to serve
     >     [Sympa SOAP interface](../customize/soap-api.md) with this method,
     >     you may have to write an rc script to start another spawn-fcgi
     >     service.  Contribution by readers will be appreciated.

  2. Start WWSympa FastCGI service.
     ```bash
     # /usr/local/etc/rc.d/spawn-fcgi start
     ```

#### System V init script

  1. If your system supports system V init script, edit
     [example init script](../examples/initscripts/wwsympa) as you prefer,
     and copy it to system V init directory (such as ``/etc/rc.d/init.d``)
     as the ``wwsympa`` file.

     > **Note**
     >
     >   * You can also serve Sympa SOAP interface with this method. Follow the
     >     same instructions but with this
     >     [example init script](../examples/initscripts/sympasoap). Copy it as
     >     the ``sympasoap`` file.

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

Proceed to
[Setup HTTP server as FastCGI proxy](configure-http-server-proxy.md).

