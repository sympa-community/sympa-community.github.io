---
title: 'Install dependent modules'
prev: install-sympa-distribution.md
up: ../install.md
next: generate-initial-configuration.md
---

Install dependent modules
=========================

Binary distributions
--------------------

If you installed binary distribution (apt, RPM, ports, ...), required
dependent modules may have been installed: You can
[skip this section](generate-initial-configuration.md).

Using cpanminus (`cpanm`)
-------------------------

> **Note**
>
>   * Support for cpanminus was introduced on Sympa 6.2.34.
>
>   * If you are using Perl _earlier than_ 5.16.0 with Sympa
>     _earlier than_ 6.2.62, in addition to
>     modules installed in this section, you have to install manually
>     [`Unicode::CaseFold`](https://metacpan.org/pod/Unicode::CaseFold)
>     which is not included in `cpanfile`, or Sympa won't work correctly.

### Requirement

  * [cpanminus](https://metacpan.org/pod/App::cpanminus) in required.

    With binary distributions, install following packages:

      - Debian/Ubuntu: `cpanminus`
      - FreeBSD package/ports: `devel/p5-App-cpanminus`
      - RPM: `perl-App-cpanminus`
      - macOS with homebrew (`brew`): `cpanminus`

    Or, run [`cpan`](https://metacpan.org/pod/cpan) command line tool
    bundled in Perl to build and install cpanminus:

    ``` bash
    # cpan App::cpanminus
    ```

    Otherwise, follow the
    [instruction](https://metacpan.org/pod/App::cpanminus#INSTALLATION)
    by the author.

### Instruction

To install (or upgrade) required and recommended dependent modules, run:

``` bash
$ cd <top of source>
# cpanm --installdeps --with-recommends .
```

To install also modules required for development tasks:

``` bash
$ cd <top of source>
# cpanm --installdeps --with-recommends --with-develop .
```

To install the latest version of specific module (even if it is not listed
in `cpanfile` file):

``` bash
# cpanm Name::Of::Module
```

For details about usage of `cpanm`, see
[the documentation](https://metacpan.org/dist/App-cpanminus/view/bin/cpanm).

Using package management tools
------------------------------

Also, you can use any package management tools on your system
(apt, yum, pkg, ...) or generic tools
([cpan](http://perldoc.perl.org/cpan.html), ...).

To know what modules you should install, see `cpanfile` file.
This file is put in the top of source tarball, and when Sympa has been
installed, it is put in [`$MODULEDIR`](../layout.md#moduledir) directory.

> **Note**
>
>   * On Sympa prior to version 6.2.34, modules to be installed were defined in
>     [``$MODULEDIR/Sympa/ModDef.pm``](/gpldoc/man/Sympa-ModDef.3.html).

Using `sympa_wizard`
--------------------
_Obsoleted method_.

> **Note**
>
>   * `sympa_wizard` will be deprecated on Sympa 6.2.70.  Use of `cpanm`
>     described in above is recommended for recent version of Sympa.

Run ``sympa_wizard`` to install dependent modules.
```
# sympa_wizard.pl --check
```
It checks your system, gets lacking or outdated modules from
[CPAN](https://www.cpan.org/) and installs them.

