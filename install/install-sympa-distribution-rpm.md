---
title: 'Install Sympa distribution: RPM package'
prev: ../requirements.md
up: install-sympa-distribution.md
next: generate-initial-configuration.md
---

Install Sympa distribution: RPM package
=======================================

Currently, [unofficial YUM repository](http://sympa-ja.org/download/rhel/)
is provided by volunteer.

Requirements
------------

* Red Hat Enterprise Linux (RHEL) 6 or 7 or its clones.
  Among clones, at least [CentOS](https://www.centos.org/download/) 6 or 7
  is reported working.

  ----
  Note:

  * Packages for RHEL/CentOS 5 will no longer be provided.

  ----

* On RHEL, the "optional repository" should be enabled: Some dependent
  packages are shipped in this repository.

Installing
----------

1. Add
   [Extra Packages for Enterprise Linux](https://fedoraproject.org/wiki/EPEL)
   (EPEL) repository.  It can be done by installing ``epel-release``
   package.  For more details see
   [EPEL description](https://fedoraproject.org/wiki/EPEL#How_can_I_use_these_extra_packages.3F).

2. Add unofficial Sympa repository.

   Download a
   [repository configuration file](http://sympa-ja.org/download/rhel/sympa-ja.org.rhel.repo)
   and save it in ``/etc/yum.repos.d`` directory.

   Then update cache:
   ```bash
   # yum makecache
   ```

3. Install Sympa:
   ```bash
   # yum install sympa
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

