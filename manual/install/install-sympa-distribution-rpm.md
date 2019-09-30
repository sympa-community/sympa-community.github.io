---
title: 'Install Sympa distribution: RPM package'
prev: ../requirements.md
up: install-sympa-distribution.md
next: generate-initial-configuration.md
---

Install Sympa distribution: RPM package
=======================================

Currently, YUM/DNF repositories provide RPM packages of Sympa.

  * Stable packages of Sympa for Fedora/RHEL/CentOS are provided.

  * Pre-release packages for Fedora/RHEL/CentOS are provided by
    [COPR ``sympa-beta`` repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa-beta/).

----
Note:

  * Packages for RHEL/CentOS 8 are work in progress.

  * If you have been using _Repository on Sympa-JA.org_, you may
    seamlessly upgrade `sympa` package with EPEL.

  * Packages for RHEL/CentOS 5 will no longer be provided.

----

Requirements
------------

  * Fedora, Red Hat Enterprise Linux (RHEL) 6 or 7 or its clones.
    Among clones, at least [CentOS](https://www.centos.org/download/) 6 or 7
    is reported working.

  * With RHEL/CentOS, add
    [Extra Packages for Enterprise Linux](https://fedoraproject.org/wiki/EPEL)
    (EPEL) repository.  It can be done by installing ``epel-release``
    package.  For more details see
    [EPEL description](https://fedoraproject.org/wiki/EPEL#How_can_I_use_these_extra_packages.3F).

  * On RHEL, in addition, the "optional repository" should be enabled:
    Some dependent packages are shipped in this repository.

Installing
----------

  1. Install Sympa:
     ```bash
     # yum install sympa
     ```
     or (if you are using `DNF`)
     ```bash
     # dnf install sympa
     ```

Thus, Sympa and dependent packages will be installed.

Upgrading
---------

  1. Update yum/DNF, dependent packages and then Sympa packages:
     ```bash
     # yum update yum
     # yum update --exclude='sympa*'
     # yum update sympa
     ```
     or (if you are using `DNF`)
     ```bash
     # dnf update dnf
     # dnf update --exclude='sympa*'
     # dnf update sympa
     ```
Installing earlier version
--------------------------

If you have to (re)install RPMs of Sympa prior to 6.2.44, they are
provided by
[repository on Sympa-JA.org](http://sympa-ja.org/download/rhel/).

----
Note:

  * Sympa-JA.org repository will no longer be updated.
    Unless you needed particular historic version of Sympa,
    follow the instruction described in above.

----

Installing
----------

  1. Install EPEL repository (see "[Requirements](#requirements)" above).

  2. Add Sympa repository.

     Download a configuration file of either repository:
     
       * [Repository on Sympa-JA.org](http://sympa-ja.org/download/rhel/sympa-ja.org.rhel.repo)

     and save it in ``/etc/yum.repos.d/`` directory.

     Then update cache:
     ```bash
     # yum makecache
     ```
     or (if you are using `DNF`)
     ```bash
     # dnf makecache
     ```

  3. Install Sympa:
     ```bash
     # yum install sympa
     ```

By the steps above, Sympa and dependent packages will be installed.
