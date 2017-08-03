Install Sympa distribution: Installing from source
==================================================

Requirements
------------

See also "[Requirements](../requirements.md)".

* [GNU tar](https://www.gnu.org/software/tar/) (sometimes called "gtar").

* ANSI-compliant C compiler,
  for example [gcc](https://gcc.gnu.org/) (GCC C Compiler)
  or [clang](http://clang.llvm.org/).

* [Perl 5 interpreter](https://www.perl.org/get.html) is required.
  Currently, Perl 5.8.1 and later are supported, however, recent version of
  Perl 5 is recommended.

* [GNU gettext](https://www.gnu.org/software/gettext/).

  ----
  Note:

  * On several binary releases, required files (``po.m4`` etc.) may be
    shipped in separate "development package" named such as ``gettext-dev``
    or ``gettext-devel``.

  ----

* make(1) utility. [GNU make](https://www.gnu.org/software/make/)
  (sometimes called "gmake") is recommended.

* If you wish to install development version, these packages are also needed:
  - [Git](https://git-scm.com/downloads).
  - [GNU autoconf](https://www.gnu.org/software/autoconf/).
  - [GNU automake](https://www.gnu.org/software/automake/).
  - [GNU gettext](https://www.gnu.org/software/gettext/) which includes
    utilities msgcat(1), msgfmt(1) and msgmerge(1).

Installation overview
---------------------

1. Create system group and user.

2. Get and unpack source.

3. Run ``configure`` script.

4. Build and install.

Create system group and user
----------------------------

At first, create a group and a user dedicated to Sympa.

### Most of Linux distributions

```
# groupadd sympa
# useradd -g sympa -c 'Sympa user' -s /sbin/nologin sympa
```

### FreeBSD

```
# pw groupadd sympa
# pw useradd sympa -g sympa -c 'Sympa user' -s /usr/sbin/nologin
```

### macOS

```
# dscl . -create /Groups/sympa gid <new GID>
# dscl . -create /Users/sympa
# dscl . -create /Users/sympa UserShell /sbin/nologin
# dscl . -create /Users/sympa RealName 'Sympa user'
# dscl . -create /Users/sympa UniqueID <new UID>
# dscl . -create /Users/sympa PrimaryGroupID <new GID>
```

Get and unpack source
---------------------

### Release version

If you wish to install release version (and we also recommend it),
download the newest source tarball from
[Sympa release page](https://github.com/sympa-community/sympa/releases).

The source tarball is named ``sympa-6.2.XX.tar.gz``.

Unpack it, and move into the new directory:
```
$ tar xzf sympa-6.2.XX.tar.gz
$ cd sympa-6.2.XX
```

### Development version

If you wish to install development version, create local clone of
[git repository](https://github.com/sympa-community/sympa.git) and checkout
``sympa-6.2`` branch:
```
$ git clone https://github.com/sympa-community/sympa.git sympa-6.2-head
$ cd sympa-6.2-head
$ git checkout -b sympa-6.2 origin/sympa-6.2
```
Once you have created your local clone, you can get the latest source at any
time just doing:
```
$ cd sympa-6.2-head
$ git pull
```

----
Note:

* If you are planning to contribute to Sympa by fixing bugs or adding
  enhancements, you would be better to create GitHub account of your own,
  "fork" original repository and work on it.  For more details see
  [GitHub documentation](https://help.github.com/articles/fork-a-repo/).

----

Run ``configure`` script
------------------------

* Note for development version: You should run ``autoreconf`` in advance:
  ```
  $ autoreconf -i
  ```

Run ``configure`` script with appropriate options:
```
$ ./configure (...options...)
```

Appropriate options are vary by situations.
Subsequent sections describe how to choose options.

### Upgrading earlier versions

If you are upgrading earlier version of Sympa, you may choose _the same_ options you specified at the time of the last build.

### New installation

If you are installing Sympa anew, ``--enable-fhs`` and ``--prefix`` options are recommended, for example:
```
$ ./configure --enable-fhs --prefix=/usr/local --with-confdir=/etc/sympa (...other options...)
```

----
Note:

* On the future releases of Sympa, the ``--enable-fhs`` option will be enabled
  by default.

----

### Other useful options

- ``--with-initdir=/etc/rc.d/init.d``

  Installs system V init scripts into specified directory.

- ``--without-initdir --with-unitsdir=/usr/lib/systemd/system``

  Installs Systemd unit files into specified directory.

- ``--without-initdir``

  Installs neither of system V init scripts nor Systemd unit files.

- ``--with-perl=/path/to/perl``

  Uses alternative Perl interpreter.

- ``--with-makemap=/path/to/makemap``,
  ``--with-newaliases=/path/to/newaliases``,
  ``--with-postalias=/path/to/postalias``,
  ``--with-postmap=/path/to/postmap``

  Specifys paths to makemap, newaliases, postalias and postmap.
  By default, they are automatically detected.

- ``--without-smrshdir``

  Does not install symbolic links only useful for Sendmail smrsh.

To know about all available options for configure script, run:
```
$ ./configure --help
```

Build and install
-----------------

At last ``configure`` script has been run successfully, run ``make``.
If it succeeded, run ``make install`` to install Sympa.
```
$ make
# make install
```

