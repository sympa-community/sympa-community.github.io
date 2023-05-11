---
title: 'Install Sympa distribution: RPM package'
prev: ../requirements.md
up: install-sympa-distribution.md
next: generate-initial-configuration.md
---

Install Sympa distribution: RPM package
=======================================

Currently, several DNF (YUM) repositories provide RPM packages of Sympa.

  * Pre-release packages for Fedora/RHEL/CentOS are provided by
    [COPR ``sympa-beta`` repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa-beta/).

> **Note**
>
>   * Packages for RHEL/CentOS 6 or earlier will no longer be provided.

Requirements
------------

  * Red Hat Enterprise Linux (RHEL) or several clones of it
    ([CentOS](https://www.centos.org/download/),
    [Rocky Linux](https://rockylinux.org/),
    [AlmaLinux](https://almalinux.org/)):

    [Extra Packages for Enterprise Linux](https://docs.fedoraproject.org/en-US/epel/)
    (EPEL) repository has to be added.  It can be done by installing ``epel-release``
    package.  For more details see
    [EPEL description](https://docs.fedoraproject.org/en-US/epel/#_quickstart).

  * [Fedora](https://getfedora.org/):

    Packages for Sympa are provided.

  * [Mageia](https://www.mageia.org/):

    Stable package for Sympa is provided.

  * Note that some distributions need enabling additional repository/ies:
    Some dependent packages are shipped in those repositories.

Installing
----------

  1. Install Sympa:
     ```bash
     # dnf install sympa
     ```
     or (if you are using `yum`)
     ```bash
     # yum install sympa
     ```

Thus, Sympa and dependent packages will be installed.

Upgrading
---------

  1. Update DNF (yum), dependent packages and then Sympa packages:
     ```bash
     # dnf update dnf
     # dnf update --exclude='sympa*'
     # dnf update sympa
     ```
     or (if you are using `yum`)
     ```bash
     # yum update yum
     # yum update --exclude='sympa*'
     # yum update sympa
     ```

Installing earlier version
--------------------------

If you have to (re)install RPMs of Sympa prior to 6.2.44, check the
[archive](http://assets.sympa.community/old-packages/).

> **Note**
>
>   * Unless you needed particular historic version of Sympa,
>     follow the instruction described in above.
