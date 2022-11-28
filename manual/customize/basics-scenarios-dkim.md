---
title: 'Authorisation scenarios: Obsoleted feature'
up: basics-scenarios.md
---

Authorisation scenarios: Obsoleted feature
==========================================

The `dkim` authentication method for scenarios
----------------------------------------------

> **Note**
>
>   * The `dkim` authentication method for scenarios was introduced
>     on Sympa 6.1, and has been deprecated after Sympa 6.2.70.
>
>     On Sympa 6.2.72 or later, the `dkim` is just a synonym of the `smtp`
>     authentication method and never has special meanings.

On Sympa 6.2.70 or earlier,
turning on the
[dkim_feature](/gpldoc/man/sympa_config.5.html#dkim_feature)
configuration parameter will also provide a new authentication level to the
scenario engine.
Scenario evaluation for incoming messages with a valid DKIM signature
(but no S/MIME signature) will be evaluated with authentication method
`dkim`. So rules that use authentication method `smtp` will not match.

The `dkim` authentication method is used in mail context but _never_
used in web context.

Example:

``` code
  is_subscriber([listname],[sender])   smtp      request_auth
  is_subscriber([listname],[sender])   md5,smime do_it
```

Those 2 rules will not match any messsage with a valid DKIM signature,
you must replace them with one of the following:

``` code
  is_subscriber([listname],[sender])   smtp,dkim request_auth
  is_subscriber([listname],[sender])   md5,smime do_it

  is_subscriber([listname],[sender])   smtp           request_auth
  is_subscriber([listname],[sender])   dkim,md5,smime do_it
```

If you choose the second solution, you accept `dkim` as a valid
authentication method.

