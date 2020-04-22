---
title: 'Configure HTTP server: Using Systemd socket'
prev: configure-mail-server.md
up: configure-http-server-.md
next: configure-http-server.md#tests
---

Configure HTTP server: Using Systemd socket
===========================================

Requirements
------------

  * Operating system must support
    [Systemd](https://freedesktop.org/wiki/Software/systemd/).

  * HTTP server.

    Currently, [nginx](https://nginx.org/en/download.html),
    [Apache HTTP Server](https://httpd.apache.org/download.cgi)
    (2.4 or later) and [lighttpd](https://www.lighttpd.net/)
    are reported working.

    ----
    Note:

      * Notes in
        "[Configure HTTP server: Using separate FastCGI service](configure-http-server-spawnfcgi.md)"
        also apply to this chapter.

    ----

  * Installing
    [multiwatch](https://redmine.lighttpd.net/projects/multiwatch/wiki)
    is strongly recommended, especially on the sites with high traffic.


General instruction
-------------------

First, install Systemd socket and WWSympa FastCGI service.  And then
configure HTTP server if necessary.

### Install Systemd socket

Prepare socket unit file.

`/lib/systemd/system/wwsympa.socket`:
``` code
[Unit]
Description = WWSympa - Web interface for Sympa mailing list manager (socket)

[Socket]
SocketUser = nginx
SocketMode = 0600
ListenStream = /run/sympa/wwsympa.socket

[Install]
WantedBy = sockets.target
```

If the socket should be read by the other user, you may add a
configuration file such as:

`/etc/systemd/system/wwsympa.socket.d/socket.conf`:
``` code
[Socket]
SocketUser=apache
```

### Install WWSympa FastCGI service

Prepare service unit file.

`/lib/systemd/system/wwsympa.service`:
``` code
[Unit]
Description = WWSympa - Web interface for Sympa mailing list manager (service)
After = syslog.target sympa.service

[Service]
User = sympa
Group = sympa
ExecStart = /usr/libexec/sympa/wwsympa.fcgi
StandardOutput = null
StandardInput = socket
StandardError = null
Restart=on-failure

[Install]
WantedBy = multi-user.target
```

Or, you might want to use multiwatch to run multiple workers.

`wwsympa.service`:
``` code
[Unit]
Description = WWSympa - Web interface for Sympa mailing list manager (service)
After = syslog.target sympa.service

[Service]
User = sympa
Group = sympa
ExecStart = /usr/local/multiwatch/bin/multiwatch \
    -f $FCGI_CHILDREN -- \
    /usr/libexec/sympa/wwsympa.fcgi
StandardOutput = null
StandardInput = socket
StandardError = null
Environment="FCGI_CHILDREN=5"
EnvironmentFile=-/etc/sysconfig/sympa
Restart=on-failure

[Install]
WantedBy = multi-user.target
```

### Setup HTTP server

Instructions in
"[Configure HTTP server: Using separate FastCGI service](configure-http-server-spawnfcgi.md)"
are also applicable to this chapter.

### Starting service

To start WWSympa service, start Systemd socket:

     ``` bash
     # systemctl start wwsympa.socket
     ```

`wwsympa.service` will be invoked automatically.

