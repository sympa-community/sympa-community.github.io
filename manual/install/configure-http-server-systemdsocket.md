---
title: 'Configure HTTP server: Using Systemd socket'
prev: configure-mail-server.md
up: configure-http-server.md
next: configure-http-server-proxy.md
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

    > **Note**
    >
    >   * Notes in
    >     "[Configure HTTP server: Using separate FastCGI service](configure-http-server-spawnfcgi.md)"
    >     also apply to this chapter.

  * Installing
    [multiwatch](https://redmine.lighttpd.net/projects/multiwatch/wiki)
    is strongly recommended, especially on the sites with high traffic.

General instruction
-------------------

> **Note**
>
>   * Do not mix the instructions in this chapter with
>     ones in the chapter
>     "[Using separate FastCGI service](configure-http-server-spawnfcgi.md)".
>     They are incompatible each other, and even though they have the same
>     name in the configuration files, their contents are different.

First, install Systemd socket and WWSympa FastCGI service.  And then
configure HTTP server if necessary.

### Install Systemd socket

Prepare socket unit file (Note:
Replace [``$PIDDIR``](../layout.md#piddir) below).

`/lib/systemd/system/wwsympa.socket`:
``` code
[Unit]
Description=Sympa web interface socket

[Socket]
SocketUser=nobody
SocketMode=0600
ListenStream=$PIDDIR/wwsympa.socket

[Install]
WantedBy=sockets.target
```

If the socket should be read by the other user than `nobody`,
you may create a drop-in snippet file such as:

`/etc/systemd/system/wwsympa.socket.d/override.conf`:
``` code
[Socket]
SocketUser=apache
```
And run:
``` bash
# systemctl daemon-reload
```

Note that you may also run `systemctl edit wwsympa.socket`
to create snippet and reload daemon at once.

### Install WWSympa FastCGI service

Prepare service unit file (Note:
Replace [``$EXECCGIDIR``](../layout.md#execcgidir) below).

`/lib/systemd/system/wwsympa.service`:
``` code
[Unit]
Description = WWSympa - Web interface for Sympa mailing list manager (service)
After = syslog.target sympa.service

[Service]
User = sympa
Group = sympa
ExecStart = $EXECCGIDIR/wwsympa.fcgi
StandardOutput = null
StandardInput = socket
StandardError = null
Restart=on-failure

[Install]
WantedBy = multi-user.target
```

Or, you might want to use multiwatch to run multiple workers (Note:
Replace [``$EXECCGIDIR``](../layout.md#execcgidir) below, but _do not_
replace ``$FCGI_CHILDREN``).

`wwsympa.service`:
``` code
[Unit]
Description=Sympa web interface FastCGI backend
After=sympa.service
Requires=wwsympa.socket

[Service]
User=sympa
Group=sympa
ExecStart=/usr/bin/multiwatch \
          -f $FCGI_CHILDREN -- \
          $EXECCGIDIR/wwsympa.fcgi
StandardOutput=null
StandardInput=socket
StandardError=journal
Environment="FCGI_CHILDREN=5"
Restart=always
RestartSec=5

[Install]
Also=wwsympa.socket
WantedBy=multi-user.target
```

By default, five processes will run.  To change number of processes,
you may create a drop-in snippet file such as:

`/etc/systemd/system/wwsympa.service.d/override.conf`:
``` code
[Service]
Environment="FCGI_CHILDREN=2"
```
And run:
``` bash
# systemctl daemon-reload
```

Note that you may also run `systemctl edit wwsympa.service`
to create snippet and reload daemon at once.

> **Note**
>
>   * Some distributions bundle unit files in their binary packages,
>     so it is recommended to use them.  Examples are:
>     RHEL/CentOS 8 or later, Debian 12 (bookworm) or later.
>
>     Since these are subject to modifications by each distribution,
>     recommended method of customization may vary.
>     Check the documentation comes with the package of each distribution.

> **Note**
>
>   * You can also serve
>     [Sympa SOAP interface](../customize/soap-api.md) with this method.
>     Follow the same instructions in this chapter but with unit files
>     ``sympasoap.socket`` and ``sympasoap.service``.

### Setup HTTP server

Proceed to
[Setup HTTP server as FastCGI proxy](configure-http-server-proxy.md).

Stopping and starting service
-----------------------------

### Stopping service

To stop WWSympa service, stop Systemd socket also:

     ``` bash
     # systemctl stop wwsympa.socket
     # systemctl stop wwsympa.service
     ```

### Starting service

To start WWSympa service, start Systemd socket:

     ``` bash
     # systemctl start wwsympa.socket
     ```

`wwsympa.service` will be invoked automatically when the socket will be accessed.
