---
title: 'Install Sympa distribution: FreeBSD package'
prev: ../requirements.md
up: install-sympa-distribution.md
next: generate-initial-configuration.md
---

Install Sympa distribution: FreeBSD package
===========================================

Using binary packages is straight-forward: installing or upgrading sympa's code is on-line:

```bash
# pkg install sympa
```

If you prefer using ports, this can be replaced by:
```bash
# pkg delete sympa (to remove old version)
# cd /usr/ports/mail/sympa
# make install
```

