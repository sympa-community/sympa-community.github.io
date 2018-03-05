---
title: 'LMTP delivery'
up: ../customize.md#sympa-and-other-systems
---

LMTP Delivery
=============

[``smtpc``](https://github.com/ikedas/smtpc) is a small program to be used as
an alternative to sendmail(1) utility.  It supports
Local Mail Transfer Protocol ([LMTP](https://tools.ietf.org/html/rfc2033))
along with SMTP / ESMTP.

Installation
------------

You can get source tarball at
[release page](https://github.com/ikedas/smtpc/releases),
then build and install:

``` bash
$ tar xzf smtpc-X.X.X.tar.gz
$ cd smtpc-X.X.X
$ ./configure --bindir=<directory to install>
$ make
# make install
```

----
Note:

  * As of Sympa 6.2.26, ``smtpc`` was no longer bundled in Sympa.

----

Setup
-----

### Requirements

  * LMTP or SMTP server to deliver messages received from ``smtpc``.

    ----
    Note:

    ``smtpc`` itself does not provide features included in mail server,
    such as mail routing, message queuing, rtrying delivery after temporary
    failure.

    ----

### Sympa configuration parameter

  * [``sendmail``](../man/sympa.conf.5.md#sendmail)

    Absolute path to sendmail command line utility. 

  * [``sendmail_args``](../man/sympa.conf.5.md#sendmail_args)

    Arguments fed to utility above, separated by spaces.

### Instruction

Add settings to [``sympa.conf``](../layout.md#config) as following (Note:
replace ``/path/to/smtpc`` with full path to ``smtpc``.

#### LMTP delivery using Unix domain socket

```
sendmail /path/to/smtpc
sendmail_args --lmtp /absolute/path/to/socket
```

Note that socket should have been created by LMTP server.

#### LMTP delivery using TCP socket

```
sendmail /path/to/smtpc
sendmail_args --lmtp server.example.org:24
```

Note that LMTP server (``server.example.org`` above) should listen specified
port. Port number may be omitted if it is default port (``24``).

#### SMTP/ESMTP delivery

```
sendmail /path/to/smtpc
sendmail_args --esmtp server.example.org:25
```

Note that LMTP server (``server.example.org`` above) should listen specified
port. Port number may be omitted if it is default port (``25``).
