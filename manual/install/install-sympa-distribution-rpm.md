---
title: 'Install Sympa distribution: RPM package'
prev: ../requirements.md
up: install-sympa-distribution.md
next: generate-initial-configuration.md
---

Install Sympa distribution: RPM package
=======================================

Currently, these YUM/DNF repositories are provided by volunteers.

  * [Repository on Sympa-JA.org](http://sympa-ja.org/download/rhel/):
    Stable packages for RHEL/CentOS 6 and 7.
  * [COPR ``sympa`` repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa/):
    Stable packages for Fedora/RHEL/CentOS.
  * [COPR ``sympa-beta`` repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa-beta/):
    Pre-release packages for Fedora/RHEL/CentOS.

If you have been using _Repository on Sympa-JA.org_, you are recommended
to use it for the recent release.
Otherwise, you are recommended to use _COPR ``sympa`` repository_.

If you wish to use pre-release (beta) versions of Sympa, use
_COPR ``sympa-beta`` repository_ which is compatible to
_COPR ``sympa`` repository_.

----
Note:

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

  1. Add Sympa repository.

     Download a configuration file of either repository:
     
       * [Repository on Sympa-JA.org](http://sympa-ja.org/download/rhel/sympa-ja.org.rhel.repo)
       * [COPR ``sympa`` repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa/)
       * [COPR ``sympa-beta`` repository](https://copr.fedorainfracloud.org/coprs/xavierb/sympa-beta/)

     and save it in ``/etc/yum.repos.d/`` directory.

     Then update cache:
     ```bash
     # yum makecache
     ```
     or (if you are using `DNF`)
     ```bash
     # dnf makecache
     ```

  2. Install Sympa:
     ```bash
     # yum install sympa
     ```
     or (if you are using `DNF`)
     ```bash
     # dnf install sympa
     ```

By the steps above, Sympa and dependent packages will be installed.

Upgrading
---------

  1. Update yum, dependent packages and then Sympa packages:
     ```bash
     # yum update yum
     # yum update --exclude='sympa*'
     # yum update sympa
     ```
     or (if you are using `DNF`)
     ```bash
     # dnf update yum
     # dnf update --exclude='sympa*'
     # dnf update sympa
     ```
