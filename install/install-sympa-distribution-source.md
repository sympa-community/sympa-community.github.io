Install Sympa distribution: Installing from source
==================================================

Requirements
------------

See also "[Requirements](requirements.md)".

* ANSI-compliant C compiler,
  for example [gcc](https://gcc.gnu.org/) (GCC C Compiler)
  or [clang](http://clang.llvm.org/).

* make(1) utility. [GNU make](https://www.gnu.org/software/make/)
  (sometimes called "gmake") is recommended.

* [Perl 5 interpreter](https://www.perl.org/get.html).
  At least Perl 5.8.1 is required,
  however, recent version of Perl 5 is recommended.

* MTA has been installed: For example Sendmail, Postfix, OpenSMTPD.
  MTA service need not have started at this time.

* If you wish to install development version, these packages are also needed:
  - [GNU autoconf](https://www.gnu.org/software/autoconf/)
  - [GNU automake](https://www.gnu.org/software/automake/)
  - [GNU gettext](https://www.gnu.org/software/gettext/), including ``msgcat``, ``msgfmt`` and ``msgmerge``.

Installation overview
---------------------

1. Create system group and user

2. Get and unpack source

3. Run ``configure`` script

4. Build and install

Create system group and user
----------------------------

At first, create a group and a user dedicated to Sympa.

For example, on most of Linux distributions:
```
# groupadd sympa
# useradd -g sympa -s /sbin/nologin sympa
```

On macOS:
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

* If you wish to install stable version (and we also recommend it),
  download the newest source tarball from Sympa release page:
  https://github.com/sympa-community/sympa/releases

  The source tarball is named ``sympa-6.2.XX.tar.gz``.

  Unpack it, and move into the new directory:
  ```
  $ zcat sympa-6.2.XX.tar.gz | tar xf -
  $ cd sympa-6.2.XX
  ```

* If you wish to install development version,
  create local clone of [git repository](https://github.com/sympa-community/sympa.git) and checkout ``sympa-6.2`` branch:
  ```
  $ git clone https://github.com/sympa-community/sympa.git sympa-6.2-head
  $ cd sympa-6.2-head
  $ git checkout -b sympa-6.2 origin/sympa-6.2
  ```

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
$ ./configure --enable-fhs --prefix=/usr/local --sysconfdir=/etc/sympa --with-confdir=/etc/sympa (...other options...)
```
On the future releases, the ``--enable-fhs`` option will be enabled by default.

### Other useful options

- ``--with-initdir=/etc/rc.d/init.d``

  Installs system V init scripts into specified directory.

- ``--without-initdir --with-unitsdir=/usr/lib/systemd/system``

  Installs Systemd unit files into specified directory.

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

