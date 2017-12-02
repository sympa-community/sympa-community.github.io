---
title: LMTP delivery
up: ../customize.md
---

LMTP Delivery
=============

[``sympa_smtpc``](../man/sympa_smtpc.1.md) is a small program to be used as
an alternative to sendmail(1) utility.  It supports
[LMTP protocol](https://tools.ietf.org/html/rfc2033) along with SMTP/ESMTP
protocols.

Installation
------------

With source distribution, ``sympa_smtpc`` will be installed by default.

With bindary distribution, it may sometimes not be included in package.
If it is the case, you can build and install it using source distribution:

``` bash
$ tar xzf sympa-6.2.XX.tar.gz
$ cd sympa-6.2.XX/src/smtpc
$ ./configure --bindir=<directory to install> --program-prefix='sympa_'
$ make
# make install
```

Setup
-----

### Requirements

  * LMTP or SMTP server to deliver messages received from ``sympa_smtpc``.

    ----
    Note:

    ``sympa_smtpc`` itself does not provide features included in mail server,
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
replace ``/path/to/sympa_smtpc`` with full path to ``sympa_smtpc``.

#### LMTP delivery using Unix domain socket

```
sendmail /path/to/sympa_smtpc
sendmail_args --lmtp /absolute/path/to/socket
```

Note that socket should have been created by LMTP server.

#### LMTP delivery using TCP socket

```
sendmail /path/to/sympa_smtpc
sendmail_args --lmtp server.example.org:24
```

Note that LMTP server (``server.example.org`` above) should listen specified
port. Port number may be omitted if it is default port (``24``).

#### SMTP/ESMTP delivery

```
sendmail /path/to/sympa_smtpc
sendmail_args --esmtp server.example.org:25
```

Note that LMTP server (``server.example.org`` above) should listen specified
port. Port number may be omitted if it is default port (``25``).
