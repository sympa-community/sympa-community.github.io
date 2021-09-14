---
title: 'Directory layout'
redirect_from:
  - ../layout.html
---

Directory layout
================

Location of configuration files, spools and executables depends on the
distribution you have installed.  Therefore, in this document, several symbols
are used to stand for particular paths.  You may have to replace uppercase
symbols with real paths on your Sympa.

----
Notes:

  * All paths below are defined in an internal module
    [``$MODULEDIR/Sympa/Constants.pm``](/gpldoc/man/Sympa-Constants.3.html).

      * Definitions of [``$EXECCGIDIR``](#execcgidir), [``$CSSDIR``](#cssdir)
        and [``$PICTURESDIR``](#picturesdir) were added to
        ``Sympa/Constants.pm`` as of Sympa 6.2.26.

  * Default paths below are determined with options fed to `configure`
    script in the time of building packages by each distribution.

      * About the "suggested configure option" in below, see
        "Run configure script" -
        "[New installation](install/install-sympa-distribution-source.md#new-installation)".

----

#### ``$CONFIG``

**Main configuration file** (``sympa.conf``).

| Distribution                | Path                             |
|-----------------------------|----------------------------------|
| Debian                      | ``/etc/sympa/sympa.conf`` or ``/etc/sympa/sympa/sympa.conf`` (see note) |
| FreeBSD                     | ``/usr/local/etc/sympa/sympa.conf`` |
| RPM                         | ``/etc/sympa/sympa.conf``        |
| Source distribution default | ``/etc/sympa/sympa.conf``        |
| (by version prior to 6.2)   | ``/etc/sympa.conf``              |
| Suggested configure option  | ``/etc/sympa/sympa.conf``        |

(Note) On Debian 8 (jessie) or earlier, ``/etc/sympa/sympa.conf`` is used.
On Debian 9 (stretch) or later, ``/etc/sympa/sympa.conf`` may also be used,
though ``/etc/sympa/sympa/sympa.conf`` is used by default.

#### ``$SENDMAIL_ALIASES``

**Aliases file**

  * This path may be overridden by
    [``sendmail_aliases``](/gpldoc/man/sympa_config.5.html#sendmail_aliases) parameter.

| Distribution                | Default path                     |
|-----------------------------|----------------------------------|
| Debian                      | ``/etc/mail/sympa/aliases``      |
| FreeBSD                     | ``/etc/mail/sympa_aliases``      |
| RPM                         | ``/var/lib/sympa/sympa_aliases`` |
| Source distribution default | ``/etc/mail/sympa_aliases``      |
| Suggested configure option  | ``/etc/mail/sympa_aliases``      |

#### ``$PIDDIR``

**Directory of PID files**.

| Distribution                | Path                             |
|-----------------------------|----------------------------------|
| Debian                      | ``/run/sympa``                   |
| FreeBSD                     | ``/var/run/sympa``               |
| RPM                         | ``/run/sympa``                   |
| (with RHEL/CentOS 6)        | ``/var/run/sympa``               |
| Source distribution default | ``/home/sympa``                  |
| Suggested configure option  | ``/usr/local/var/run/sympa``     |

#### ``$EXPLDIR``

**List home**.  Base directory of list configurations.

  * This path may be overridden by
    [``home``](/gpldoc/man/sympa_config.5.html#home) parameter.

| Distribution                | Default path                     |
|-----------------------------|----------------------------------|
| Debian                      | ``/var/lib/sympa/list_data`` |
| FreeBSD                     | ``/usr/local/share/sympa/list_data`` |
| RPM                         | ``/var/lib/sympa/list_data``     |
| Source distribution default | ``/home/sympa/list_data``        |
| (by version prior to 6.0)   | ``/home/sympa/expl``             |
| Suggested configure option  | ``/usr/local/var/lib/sympa/list_data`` |

#### ``$SPOOLDIR``

**Base directory of spools**.

  * Location of each spool may be overridden by
    [queue](/gpldoc/man/sympa_config.5.html#queue),
    [queueauth](/gpldoc/man/sympa_config.5.html#queueauth),
    [queueautomatic](/gpldoc/man/sympa_config.5.html#queueautomatic),
    [queuebounce](/gpldoc/man/sympa_config.5.html#queuebounce),
    [queuebulk](/gpldoc/man/sympa_config.5.html#queuebulk),
    [queuedigest](/gpldoc/man/sympa_config.5.html#queuedigest),
    [queuemod](/gpldoc/man/sympa_config.5.html#queuemod),
    [queueoutgoing](/gpldoc/man/sympa_config.5.html#queueoutgoing),
    [queuesubscribe](/gpldoc/man/sympa_config.5.html#queuesubscribe),
    [queuetask](/gpldoc/man/sympa_config.5.html#queuetask),
    [queuetopic](/gpldoc/man/sympa_config.5.html#queuetopic),
    [tmpdir](/gpldoc/man/sympa_config.5.html#tmpdir) or
    [viewmail_dir](/gpldoc/man/sympa_config.5.html#viewmail_dir) parameter.

| Distribution                | Path                             |
|-----------------------------|----------------------------------|
| Debian                      | ``/var/spool/sympa`` |
| FreeBSD                     | ``/var/spool/sympa``             |
| RPM                         | ``/var/spool/sympa``             |
| Source distribution default | ``/home/sympa/spool``            |
| Suggested configure option  | ``/usr/local/var/spool/sympa``   |

#### ``$SYSCONFDIR``

**Base directory of global configuration** (except ``sympa.conf``).

  * This path may be overridden by
    [``etc``](/gpldoc/man/sympa_config.5.html#etc) parameter.

| Distribution                | Default path                     |
|-----------------------------|----------------------------------|
| Debian                      | ``/etc/sympa``                   |
| FreeBSD                     | ``/usr/local/etc/sympa``         |
| RPM                         | ``/etc/sympa``                   |
| Source distribution default | ``/home/sympa/etc``              |
| Suggested configure option  | ``/usr/local/etc``               |

#### ``$LOCALEDIR``

**Base directory of message catalogs**.

| Distribution                | Path                             |
|-----------------------------|----------------------------------|
| Debian                      | ``/usr/share/locale``            |
| (by Debian 9 (stretch) or earlier) | ``/usr/lib/sympa/locale`` |
| FreeBSD                     | ``/usr/local/share/locale``      |
| RPM                         | ``/usr/share/locale``            |
| Source distribution default | ``/home/sympa/locale``           |
| Suggested configure option  | ``/usr/local/share/locale``      |

#### ``$LIBEXECDIR``

**Path of auxiliary programs**.

| Distribution                | Path                             |
|-----------------------------|----------------------------------|
| Debian                      | ``/usr/lib/sympa/bin``           |
| FreeBSD                     | ``/usr/local/libexec/sympa``     |
| RPM                         | ``/usr/libexec/sympa``           |
| Source distribution default | ``/home/sympa/bin``              |
| Suggested configure option  | ``/usr/local/libexec``           |

#### ``$SBINDIR``

**Path of daemons and administration programs**.

| Distribution                | Path                             |
|-----------------------------|----------------------------------|
| Debian                      | ``/usr/lib/sympa/bin``           |
| FreeBSD                     | ``/usr/local/libexec/sympa``     |
| RPM                         | ``/usr/sbin``                    |
| Source distribution default | ``/home/sympa/bin``              |
| Suggested configure option  | ``/usr/local/sbin``              |

#### ``$SCRIPTDIR``

**Path of other executables**.

| Distribution                | Path                             |
|-----------------------------|----------------------------------|
| Debian                      | ``/usr/share/sympa/bin``         |
| FreeBSD                     | ``/usr/local/libexec/sympa``     |
| RPM                         | ``/usr/share/sympa/bin``         |
| Source distribution default | ``/home/sympa/bin``              |
| Suggested configure option  | ``/usr/local/share/sympa/bin``   |

#### ``$MODULEDIR``

**Base directory of internal modules** (module path).

| Distribution                | Path                             |
|-----------------------------|----------------------------------|
| Debian                      | ``/usr/share/sympa/lib``         |
| FreeBSD                     | ``/usr/local/libexec/sympa``     |
| RPM                         | ``/usr/share/sympa/lib``         |
| Source distribution default | ``/home/sympa/bin``              |
| Suggested configure option  | ``/usr/local/share/sympa/lib``   |

#### ``$DEFAULTDIR``

**Base directory of default settings**.

| Distribution                | Path                             |
|-----------------------------|----------------------------------|
| Debian                      | ``/usr/share/sympa/default``     |
| FreeBSD                     | ``/usr/local/share/sympa/defaults`` |
| RPM                         | ``/usr/share/sympa/default``     |
| Source distribution default | ``/home/sympa/default``          |
| (by version prior to 6.0)   | ``/home/sympa/bin/etc``             |
| Suggested configure option  | ``/usr/local/share/sympa/default``  |

#### ``$ARCDIR``

**Base directory of archives**.

  * This path may be overridden by
    [``arc_path``](/gpldoc/man/sympa_config.5.html#arc_path) parameter.

| Distribution                | Default path                     |
|-----------------------------|----------------------------------|
| Debian                      | ``/var/lib/sympa/arc``           |
| FreeBSD                     | ``/usr/local/share/sympa/arc``   |
| RPM                         | ``/var/lib/sympa/arc``           |
| Source distribution default | ``/home/sympa/arc``              |
| Suggested configure option  | ``/usr/local/var/lib/sympa/arc`` |

#### ``$BOUNCEDIR``

**Base directory of bounce and tracking information**.

  * This path may be overridden by
    [``bounce_path``](/gpldoc/man/sympa_config.5.html#bounce_path) parameter.

| Distribution                | Default path                     |
|-----------------------------|----------------------------------|
| Debian                      | ``/var/lib/sympa/bounce``        |
| FreeBSD                     | ``/usr/local/share/sympa/bounce`` |
| RPM                         | ``/var/lib/sympa/bounce``        |
| Source distribution default | ``/home/sympa/bounce``           |
| Suggested configure option  | ``/usr/local/var/lib/sympa/bounce`` |

Directories specific to web interface
-------------------------------------

#### ``$EXECCGIDIR``

**Path of web interface programs** (WWSympa and SympaSOAP).

| Distribution                | Path                         |
|-----------------------------|------------------------------|
| Debian                      | ``/usr/lib/cgi-bin/sympa``   |
| FreeBSD                     | ``/usr/local/libexec/sympa`` |
| RPM                         | ``/usr/libexec/sympa``       |
| Source distribution default | ``/home/sympa/bin``          |
| Suggested configure option  | ``/usr/local/lib/sympa/cgi`` |

#### ``$STATICDIR``

**Base path of static web contents**.

  * This path may be overridden by
    [``static_content_path``](/gpldoc/man/sympa_config.5.html#static_content_path)
    parameter.

| Distribution                | Default path                      |
|-----------------------------|-----------------------------------|
| Debian                      | ``/usr/share/sympa/static_content``|
| (by Debian 9 (stretch) or earlier) | ``/var/lib/sympa/static_content`` |
| FreeBSD                     | ``/usr/local/share/sympa/static`` |
| RPM                          | ``/usr/share/sympa/static_content`` |
| (by version prior to 6.2.26) | ``/var/lib/sympa/static_content``   |
| Source distribution default | ``/home/sympa/static_content``    |
| Suggested configure option  | ``/usr/local/var/lib/sympa/static_content`` |

#### ``$CSSDIR``

**Directory of automatically generated cascading style sheets (CSS)**.

  * This path may be overridden by
    [``css_path``](/gpldoc/man/sympa_config.5.html#css_path)
    parameter.

| Distribution                | Default path                          |
|-----------------------------|---------------------------------------|
| (by version prior to 6.2.26 of any distributions) | [``$STATICDIR``](#staticdir)``/css`` |
| Debian                      | ``/var/lib/sympa/css``                |
| FreeBSD                     | ``/usr/local/share/sympa/static/css`` |
| RPM                         | ``/var/lib/sympa/css``                |
| Source distribution default | ``/home/sympa/static_content/css``    |
| Suggested configure option  | ``/usr/local/var/lib/sympa/static_content/css`` |

#### ``$PICTURESDIR``

**Directory for subscribers pictures**.

  * On Sympa 6.2.26 or later, this path may be overridden by
    [``pictures_path``](/gpldoc/man/sympa_config.5.html#pictures_path)
    parameter.

  * On Sympa prior to 6.2.26, this path is a subdirectory `pictures` under the
    path that may be overridden by
    [``static_content_path``](/gpldoc/man/sympa_config.5.html#static_content_path)
    parameter.

| Distribution                | Default path                               |
|-----------------------------|--------------------------------------------|
| (by version prior to 6.2.26 of any distributions) | [``$STATICDIR``](#staticdir)``/pictures`` |
| Debian                      | TBD |
| FreeBSD                     | ``/usr/local/share/sympa/static/pictures`` |
| RPM                         | ``/var/lib/sympa/pictures``                |
| Source distribution default | ``/home/sympa/static_content/pictures``    |
| Suggested configure option  | ``/usr/local/var/lib/sympa/static_content/pictures`` |


*[Suggested configure option]: See the note above.

