Generate initial configuration
==============================

General instruction
-------------------

Edit [``sympa.conf``](../layout.md#config) so that it will contain at least
two lines such as:
```
domain mail.example.org
```
```
listmaster your@e-mail.addr.ess,other@email.addr.ess
```

* [``domain``](../man/sympa.conf.5.md#domain) is the "primary" mail domain
  name of the mailing list service.  Listmaster(s) of this mail domain name
  have privileges to manage all domains provided by your Sympa
  (a.k.a. "super-listmaster").
  If your Sympa provides service for single domain, it is primary domain.

* [``listmaster``](../man/sympa.conf.5.md#listmaster) is real address(es) of
  (super) listmasters.  Multiple addresses have to be separated by comma
  (``,``).  Addresses of real people should be chosen: Especially, messages
  sent to those addresses must not be forwarded to any addresses managed by
  Sympa.

To know about all available parameters in sympa.conf,
see [sympa.conf(5)](../man/sympa.conf.5.md).

