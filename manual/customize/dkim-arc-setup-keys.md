---
title: 'DKIM and ARC: Setup DNS resource records'
up: dkim-arc.md
next: dkim-arc-setup-mta.md
---

DKIM and ARC: Setup key pair
============================

  * Steps described in this chapter are required to support DKIM signing
    feature ("outgoing DKIM") and/or ARC feature.
    If you are not planning to support neither of them,
    [skip this chapter](dkim-arc-setup-sympa.md).

In this chapter, you have to choose some parameters:
**selector**, **signer domain** and optional **identity**.
Using the selector and signer domain, you have to generate a pair of
**private key** and **public key**.

The signer domain should be the mail domain of the list. This is the
default.  Don't change it unless you have strong reason for it.

The private key is a PEM encoded RSA key (a PEM encoded key include BASE64
encoded informations and starts with `-----BEGIN RSA PRIVATE KEY-----`).
The public key associated with that private key must be published in a DNS
TXT record for entry `<selector>._domainkey.<signer domain>`.

Example with selector "`lists`" and signer domain "`mail.example.org`":

``` code
  lists._domainkey.mail.example.org. IN TXT "v=DKIM1; g=*; k=rsa; t=y; p=MDB34............DB"
```

In order to generate the public and private keys, you may use appropriate
tool such as `openssl`, `opendkim-genkey` included in
[OpenDKIM software](http://opendkim.org/) and so on.

There are also online tools to generate them, such as
[this](https://www.socketlabs.com/domainkey-dkim-generation-wizard/).
But those services will generate the private key for you ---
is it still a private key?.
<!--
Gone.
  * http://www.port25.com/support/support_dkwz.php
-->

----

Note:

  * Currently, Sympa cannot handle the private key encrypted with a passphase.

----

