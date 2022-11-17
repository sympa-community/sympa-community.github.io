---
title: 'DKIM and ARC: Setup key pair'
up: dkim-arc.md
prev: dkim-arc.md
---

DKIM and ARC: Setup key pair
============================

  * Steps described in this chapter are required to support DKIM signing
    feature (for outgoing messages) and/or ARC feature.
    If you are not planning to support neither of them,
    [skip this chapter](dkim-arc-setup-sympa.md).

In this chapter, you have to choose some parameters:
**selector**, **signer domain** and optional **identity**.
And you have to generate a pair of **private key** and **public key**.
Then, using the selector and signer domain above,
you have to publish the public key as a DNS resource record.

> **Note**
>
>   * If you enable ARC feature, DKIM signing is also possible.
>     In that case you either may generate separate key pairs for each ARC and
>     DKIM signing, or may share the single key pair for the both.

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
tool such as `openssl` (see
[here](https://tools.ietf.org/html/rfc4871#appendix-C)),
`certtool` (included in [GnuTLS](https://www.gnutls.org/)),
`opendkim-genkey` (included in [OpenDKIM](http://www.opendkim.org/))
and so on
(There are also online tools to generate them, such as
[this](https://www.socketlabs.com/domainkey-dkim-generation-wizard/).
But those services will generate the private key for you ---
is it still a private key?).
<!--
Gone.
  * http://www.port25.com/support/support_dkwz.php
-->

----

Note:

  * Currently, Sympa cannot handle the private key encrypted with a
    passphase (password).

----

Please refer to your nameserver documentation for specific instructions on
how to register your key with the DNS.

----

After you finished setting up DNS record,

  * If you want ARC feature enabled, proceed to
    [setup MTA](dkim-arc-setup-mta.md).
  * Otherwise, proceed to [setup Sympa](dkim-arc-setup-sympa.md).

