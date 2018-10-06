---
title: 'DKIM features for Sympa'
up: ../customize.md#sympa-services-optional-features
---

DKIM features for Sympa
=======================

  * DKIM has been introduced in Sympa version **6.1**.
  * ARC has been introduced in Sympa version ^^6.2**.


DKIM is a crytographic signature method designed to prevent phishing. As postmaster or listmaster you should consider 1) checking the DKIM status of each incoming message and 2) signing all or a subset of outgoing messages.

ARC is intended to fix the problems introduced by [DMARC](dmarc-protection.md) by adding signature "seals" that show the chain of servers that processed a message.  Once ARC is more widely implemented, DMARC workarounds shouldn't be needed.  Once you have DKIM set up, adding ARC seals is straightforward.

Processing of DKIM status of incoming messages is done by the MTA (Message transfert Agent) that delivers emails to the Sympa server Sympa domain (in order to reject some messages). This topic is not addressed in the chapter of the documentation.

The mailing list server can take advantage of incoming DKIM signature in order to measure the trust of the message while evaluating message workflow. This is based on [scenario mechanism](basics-scenarios.md). An authentication level named `dkim` can be used within scenario rules to check that an incoming message has a valid DKIM signature (dkim signature status = pass).

In addition, you must consider signature of outgoing messages. Should messages brodcasted by Sympa to list subscribers be signed by your organization? Should all of them be signed? Should a subset of trusted messages be signed? Should service messages (automatic answer, welcome messages etc) be signed ?  In most cases Sympa should sign all the mail it sends to get the most benefit from DKIM.

Prerequisites
-------------

DKIM features in Sympa are based on the **[Mail::DKIM](https://metacpan.org/release/Mail-DKIM)** cpan module ; you should install it first. Check [the documentation related to cpan modules installation](../install/install-dependent-modules.md).

ARC requires Mail::DKIM version 0.60 or later which includes ARC support.  It also requires that the MTA that delivers emails to Sympa adds an `Authentication-Results:` header that shows how a message was (or wasn't) authenticated as it arrived.  Several Postfix milters can add the header, with opendmarc or the [Fastmail authentication milter](https://github.com/fastmail/authentication_milter) being widely used.


Incoming messages
-----------------

To make Sympa check the DKIM signature of incoming messages, you need to set the [dkim_feature](../man/sympa.conf.5.md#dkim_feature) configuration parameter to `on`. Before doing that you must first update your customized scenario to introduce `dkim` authentication method, **otherwise Sympa may reject messages because they include a valid DKIM signature !**. All default scenario starting at version 6.1 already include rules for DKIM, both for command and lists messages.

**What kind of changes is required in scenarios?**

Turning on the [dkim_feature](../man/sympa.conf.5.md#dkim_feature) configuration parameter will provide a new authentication level to the scenario engine. Scenario evaluation for incoming messages with a valid DKIM signature (but no S/MIME signature) will be evaluated with authentication method `dkim`. So rules that use authentication method `smtp` will not match.

Example:

``` code
  is_subscriber([listname],[sender])   smtp      request_auth
  is_subscriber([listname],[sender])   md5,smime do_it
```

Those 2 rules will not match any messsage with a valid DKIM signature, you must replace them with one of the following:

``` code
  is_subscriber([listname],[sender])   smtp,dkim request_auth
  is_subscriber([listname],[sender])   md5,smime do_it

  is_subscriber([listname],[sender])   smtp           request_auth
  is_subscriber([listname],[sender])   dkim,md5,smime do_it
```

If you choose the second solution, you accept DKIM as a valid authentication mecanism.

If the front MTA adds the [`Authentication-results`](https://tools.ietf.org/html/rfc7001) header field, Sympa can of course check this SMTP header field using standard match() and equal() scenario conditions.

Outgoing messages
-----------------

You may want to make Sympa sign outgoing messages. Almost every aspects of DKIM signature behavior can be customized via Sympa configuration parameters. Please check the [DKIM parameters section](../man/sympa.conf.5.md#dkim) for further details. Note that each parameter can also be set for a given virtual robot; and most of them are available as list parameter.

### Which messages should be signed

In order to configure Sympa for signing outgoing messages, you have to decide **which messages Sympa should sign** . This should be decided for four kind of messages:

  - Services messages : these are all messages sent by Sympa itself : welcome messages, answers to mail commands, various notification such as *remind message* and digest messages;

  - List messages : messages distributed to list members (where the initial `From:` header is preserved). These messages will fall is one one the following subcategory:

      - authenticated messages (using S/MIME signature, challenge or password);

      - received with a valid DKIM signature;

      - validated by one of the list moderators;

      - other messages.

This behavior is controlled by [dkim_add_signature_to](../man/sympa.conf.5.md#dkim_add_signature_to) and [dkim_signature_apply_on](../man/sympa.conf.5.md#dkim_signature_apply_on) parameters.

### Prerequisites for DKIM signing

Before Sympa is able to DKIM-sign messages, you need to set several related parameters. The most important ones are [dkim_private_key_path](../man/sympa.conf.5.md#dkim_private_key_path) (private key file location) and [dkim_selector](../man/sympa.conf.5.md#dkim_selector). Other parameters related to [RFC 6376](https://tools.ietf.org/html/rfc6376): [dkim_signer_domain](../man/sympa.conf.5.md#dkim_signer_domain), [dkim_signer_identity](../man/sympa.conf.5.md#dkim_signer_identity),[dkim_header_list](../man/sympa.conf.5.md#dkim_header_list).

The private key is a PEM encoded RSA key <sup><a href="#fn__1" id="fnt__1" class="fn_top">1)</a></sup> (a PEM encoded key include base64 encoded informations and starts with `—–BEGIN RSA PRIVATE KEY—–`.). The public key associated with that private key must be published in a DNS TXT record for entry `<selector>._domainkey.<domain>` where `<selector>` is [dkim_selector](../man/sympa.conf.5.md#dkim_selector) and `<domain>` is [dkim_signer_domain](../man/sympa.conf.5.md#dkim_signer_domain). The signer domain should be the domain of the list ; this is the default, don't change it unless you have strong reason for it.

example with selector = 'lists' and domain 'sympa.org':

``` code
  lists._domainkey.sympa.org. IN TXT "v=DKIM1; g=*; k=rsa; t=y; p=MDB34............DB"
```

In order to generate the public and private keys, you may use `openssl` or `dkim-genkey` included in [milter_dkim software](http://sourceforge.net/projects/dkim-milter/). There are also online tools to generate them, but those services will generate the private key for you (is it still a private key?).

  * http://www.socketlabs.com/services/dkwiz
  * http://www.port25.com/support/support_dkwz.php

Summary of parameters
---------------------

| parameter name ([``sympa.conf``](../layout.md#config) or ``robot.conf`` context) | default | overwritten by (list configuration) |
|---|---|---|
| [dkim_feature](../man/sympa.conf.5.md#dkim_feature) | `off` | not pertinent |
| [dkim_add_signature_to](../man/sympa.conf.5.md#dkim_add_signature_to) | `list,robot` | not pertinent |
| [dkim_signature_apply_on](../man/sympa.conf.5.md#dkim_signature_apply_on) | `md5_authenticated_messages,` `smime_authenticated_messages,` `dkim_authenticated_messages,` `editor_validated_messages` | `dkim_signature_apply_on` |
| [dkim_private_key_path](../man/sympa.conf.5.md#dkim_private_key_path) | | dkim > `key_path` |
| [dkim_signer_domain](../man/sympa.conf.5.md#dkim_signer_domain) | the robot domain | dkim > `signer_domain` |
| [dkim_selector](../man/sympa.conf.5.md#dkim_selector) | no default | dkim > `selector` |
| [dkim_signer_identity](../man/sympa.conf.5.md#dkim_signer_identity) | none for robot's messages, _`listname`_`-request@robot` for lists | dkim > `identity_domain` |
| ~~[dkim_header_list](../man/sympa.conf.5.md#dkim_header_list)~~ | as recommended in RFC 6376 | Not yet implemented |

<sup><a href="#fnt__1" id="fn__1" class="fn_bot">1)</a></sup> The private key can't be encrypted with a passphase
