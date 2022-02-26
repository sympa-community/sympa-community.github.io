---
title: 'Install Sympa distribution: RPM package'
prev: ../requirements.md
up: install-sympa-distribution.md
next: generate-initial-configuration.md
---

Install Sympa distribution: RPM package
=======================================

Currently, DNF (YUM) repositories provide RPM packages of Sympa.

  * Stable packages of Sympa for RHEL/CentOS are provided by EPEL, and packages for Fedora are also provided.

  * Pre-release packages for Fedora/RHEL/CentOS are provided by
    [COPR ``sympa-beta`` repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa-beta/).

----
Note:

  * Packages for RHEL/CentOS 8 and 9 are work in progress.

  * Packages for RHEL/CentOS 6 or earlier will no longer be provided.

----

Requirements
------------

  * Fedora, Red Hat Enterprise Linux (RHEL) 7 or its clones.
    Among clones, at least [CentOS](https://www.centos.org/download/) 7
    is reported working.

  * With RHEL/CentOS, add
    [Extra Packages for Enterprise Linux](https://docs.fedoraproject.org/en-US/epel/)
    (EPEL) repository.  It can be done by installing ``epel-release``
    package.  For more details see
    [EPEL description](https://docs.fedoraproject.org/en-US/epel/#_quickstart).
    
    Note that some distributions need enabling additional repository/ies:
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

If you have to (re)install RPMs of Sympa prior to 6.2.44, they are
provided by
[repository on Sympa-JA.org](http://sympa-ja.org/download/rhel/).

----
Note:

  * Sympa-JA.org repository will no longer be updated.
    And _it may be shut down_ in the near future.
    Unless you needed particular historic version of Sympa,
    follow the instruction described in above.

  * If you have been using Sympa-JA.org repository, you may
    seamlessly upgrade `sympa` package with EPEL.

----

  1. Install EPEL repository (see "[Requirements](#requirements)" above).

  2. Add Sympa repository.

     Download a configuration file of either repository:
     
       * [Repository on Sympa-JA.org](http://sympa-ja.org/download/rhel/sympa-ja.org.rhel.repo)

     and save it in ``/etc/yum.repos.d/`` directory.

     Then update cache:
     ```bash
     # yum makecache
     ```

  3. Install Sympa:
     ```bash
     # yum install sympa
     ```

By the steps above, Sympa and dependent packages will be installed.
