Directory layout
================

Location of configuration files, spools and executables depends on the
distribution you have installed.  Therefore, in this document, several symbols
are used to stand for particular paths.  You may have to replace uppercase
symbols with real paths on your Sympa.

#### ``$CONFIG``

**Main configuration file** (``sympa.conf``).

| Distribution                | Path                             |
|-----------------------------|----------------------------------|
| Debian                      | ``/etc/sympa/sympa.conf``        |
| FreeBSD                     | ``/usr/local/etc/sympa/sympa.conf`` |
| RPM                         | ``/etc/sympa/sympa.conf``        |
| Source distribution default | ``/etc/sympa/sympa.conf``        |
| [Suggested configure option](install/install-sympa-distribution-source.md#new-installation) | ``/etc/sympa/sympa.conf`` |

#### ``$SENDMAIL_ALIASES``

**Aliases file**

* This path may be overridden by
  [``sendmail_aliases``](man/sympa.conf.5.md#sendmail_aliases) parameter.

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
| RPM                         | ``/var/run/sympa``               |
| Source distribution default | ``/home/sympa``                  |
| Suggested configure option  | ``/usr/local/var/run/sympa``     |

#### ``$EXPLDIR``

**List home**.  Base directory of list configurations.

* This path may be overridden by
  [``home``](man/sympa.conf.5.md#home) parameter.

| Distribution                | Default path                     |
|-----------------------------|----------------------------------|
| Debian                      | ???? |
| FreeBSD                     | ``/usr/local/share/sympa/list_data`` |
| RPM                         | ``/var/lib/sympa/list_data``     |
| Source distribution default | ``/home/sympa/list_data``        |
| (by earlier releases)       | ``/home/sympa/expl``             |
| Suggested configure option  | ``/usr/local/var/lib/sympa/list_data`` |

#### ``$SPOOLDIR``

**Base directory of spools**.

Location of each spool may be overridden by
[queue](man/sympa.conf.5.md#queue),
[queueauth](man/sympa.conf.5.md#queueauth),
[queueautomatic](man/sympa.conf.5.md#queueautomatic),
[queuebounce](man/sympa.conf.5.md#queuebounce),
[queuebulk](man/sympa.conf.5.md#queuebulk),
[queuedigest](man/sympa.conf.5.md#queuedigest),
[queuemod](man/sympa.conf.5.md#queuemod),
[queueoutgoing](man/sympa.conf.5.md#queueoutgoing),
[queuesubscribe](man/sympa.conf.5.md#queuesubscribe),
[queuetask](man/sympa.conf.5.md#queuetask),
[queuetopic](man/sympa.conf.5.md#queuetopic),
[tmpdir](man/sympa.conf.5.md#tmpdir) or
[viewmail_dir](man/sympa.conf.5.md#viewmail_dir) parameter.

| Distribution                | Path                             |
|-----------------------------|----------------------------------|
| Debian                      | ???? |
| FreeBSD                     | ``/var/spool/sympa``             |
| RPM                         | ``/var/spool/sympa``             |
| Source distribution default | ``/home/sympa/spool``            |
| Suggested configure option  | ``/usr/local/var/spool/sympa``   |

#### ``$SYSCONFDIR``

**Base directory of global configuration** (except ``sympa.conf``).

* This path may be overridden by [``etc``](man/sympa.conf.5.md#etc) parameter.

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
| Debian                      | ???? |
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
| Suggested configure option | ``/usr/local/share/sympa/default'`` |

#### ``$STATICDIR``

**Base path of static web contents**.

* This path may be overridden by
  [``static_content_path``](man/sympa.conf.5.md#static_content_path) parameter.

| Distribution                | Default path                     |
|-----------------------------|----------------------------------|
| Debian                      | ???? |
| FreeBSD                     | ???? |
| RPM                         | ``/var/lib/sympa/static_content`` |
| Source distribution default | ``/home/sympa/static_content``   |
| Suggested configure option  | ``/usr/local/var/lib/sympa/static_content`` |

#### ``$ARCDIR``

**Base directory of archives**.

* This path may be overridden by
  [``arc_path``](man/sympa.conf.5.md#arc_path) parameter.

| Distribution                | Default path                     |
|-----------------------------|----------------------------------|
| Debian                      | ???? |
| FreeBSD                     | ???? |
| RPM                         | ``/var/lib/sympa/arc``           |
| Source distribution default | ``/home/sympa/arc``              |
| Suggested configure option  | ``/usr/local/var/lib/sympa/arc`` |

#### ``$BOUNCEDIR``

**Base directory of bounce and tracking information**.
 
* This path may be overridden by
  [``bounce_path``](man/sympa.conf.5.md#bounce_path) parameter.

| Distribution                | Default path                     |
|-----------------------------|----------------------------------|
| Debian | ???? |
| FreeBSD| ???? |
| RPM                         | ``/var/lib/sympa/bounce``        |
| Source distribution default | ``/home/sympa/bounce``           |
| Suggested configure option  | ``/usr/local/var/lib/sympa/bounce`` |

----
Note:

* All paths above are defined in an internal module
  [``$MODULEDIR/Sympa/Constants.pm``](man/Sympa-Constants.3.md).

----

