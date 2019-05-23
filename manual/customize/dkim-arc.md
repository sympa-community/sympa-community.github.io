---
title: 'DKIM and ARC features for Sympa'
up: ../customize.md#sympa-services-optional-features
redirect_from:
  - dkim.html
---

DKIM and ARC features for Sympa
===============================

  * DKIM has been introduced in Sympa version **6.1**.
  * ARC has been introduced in Sympa version **6.2.38**.

DKIM is a crytographic signature method designed to prevent phishing. As postmaster or listmaster you should consider 1) checking the DKIM status of each incoming message and 2) signing all or a subset of outgoing messages.

ARC is intended to fix the problems introduced by [DMARC](dmarc-protection.md) by adding signature "seals" that show the chain of servers that processed a message.  Once ARC is more widely implemented, DMARC workarounds shouldn't be needed.  Once you have DKIM set up, adding ARC seals is straightforward.

Processing of DKIM status of incoming messages is done by the MTA (Message transfert Agent) that delivers emails to the Sympa server Sympa domain (in order to reject some messages). This topic is not addressed in the chapter of the documentation.

The mailing list server can take advantage of incoming DKIM signature in order to measure the trust of the message while evaluating message workflow. This is based on [scenario mechanism](basics-scenarios.md). An authentication level named `dkim` can be used within scenario rules to check that an incoming message has a valid DKIM signature (dkim signature status = pass).

In addition, you must consider signature of outgoing messages. Should messages brodcasted by Sympa to list subscribers be signed by your organization? Should all of them be signed? Should a subset of trusted messages be signed? Should service messages (automatic answer, welcome messages etc) be signed ?  In most cases Sympa should sign all the mail it sends to get the most benefit from DKIM.

ARC seals only make sense on mail forwarded by Sympa, that is, individual messages sent through to mailing lists. The ARC feature will only add seals to those messages.  ARC checks the ARC seals, if any, on incoming messages, 

Prerequisites
-------------

### Prerequisites for DKIM

DKIM features in Sympa are based on the **[Mail::DKIM](https://metacpan.org/release/Mail-DKIM)** cpan module ; you should install it first. Check [the documentation related to cpan modules installation](../install/install-dependent-modules.md).

### Prerequisites for ARC

ARC requires Mail::DKIM version 0.55 or later which includes ARC support.

It also requires that the MTA that delivers emails to Sympa adds an `Authentication-Results:` header that shows how a message was (or wasn't) authenticated as it arrived.

  * Several Postfix milters can add the header, with opendmarc or the [Fastmail authentication milter](https://github.com/fastmail/authentication_milter) being widely used. 
  * [amavisd-new](https://www.ijs.si/software/amavisd/) also may add this field.

Incoming messages DKIM
----------------------

To make Sympa check the DKIM signature of incoming messages, you need to set the [dkim_feature](/gpldoc/man/sympa.conf.5.html#dkim_feature) configuration parameter to `on`. Before doing that you must first update your customized scenario to introduce `dkim` authentication method, **otherwise Sympa may reject messages because they include a valid DKIM signature !**. All default scenario starting at version 6.1 already include rules for DKIM, both for command and lists messages.

**What kind of changes is required in scenarios?**

Turning on the [dkim_feature](/gpldoc/man/sympa.conf.5.html#dkim_feature) configuration parameter will provide a new authentication level to the scenario engine. Scenario evaluation for incoming messages with a valid DKIM signature (but no S/MIME signature) will be evaluated with authentication method `dkim`. So rules that use authentication method `smtp` will not match.

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

Outgoing messages DKIM
----------------------

You may want to make Sympa sign outgoing messages. Almost every aspects of DKIM signature behavior can be customized via Sympa configuration parameters. Please check the [DKIM parameters section](/gpldoc/man/sympa.conf.5.html#dkim-and-arc) for further details. Note that each parameter can also be set for a given virtual robot; and most of them are available as list parameters.

### Which messages should be signed

In order to configure Sympa for signing outgoing messages, you have to decide **which messages Sympa should sign** . This should be decided for four kind of messages:

  - Services messages : these are all messages sent by Sympa itself : welcome messages, answers to mail commands, various notification such as *remind message* and digest messages;

  - List messages : messages distributed to list members (where the initial `From:` header is preserved). These messages will fall info one of these subcategories:

      - authenticated messages (using S/MIME signature, challenge or password);

      - received with a valid DKIM signature;

      - validated by one of the list moderators;

      - other messages.

This behavior is controlled by [dkim_add_signature_to](/gpldoc/man/sympa.conf.5.html#dkim_add_signature_to) and [dkim_signature_apply_on](/gpldoc/man/sympa.conf.5.html#dkim_signature_apply_on) parameters.

In most cases Sympa should sign **all** outgoing messages to get the maximum advantage from DKIM.

### Prerequisites for DKIM signing

Before Sympa is able to DKIM-sign messages, you need to set several related parameters. The most important ones are [dkim_private_key_path](/gpldoc/man/sympa.conf.5.html#dkim_private_key_path) (private key file location) and [dkim_selector](/gpldoc/man/sympa.conf.5.html#dkim_selector). Other parameters related to [RFC 6376](https://tools.ietf.org/html/rfc6376): [dkim_signer_domain](/gpldoc/man/sympa.conf.5.html#dkim_signer_domain), [dkim_signer_identity](/gpldoc/man/sympa.conf.5.html#dkim_signer_identity),[dkim_header_list](/gpldoc/man/sympa.conf.5.html#dkim_header_list).

The private key is a PEM encoded RSA key <sup><a href="#fn__1" id="fnt__1" class="fn_top">1)</a></sup> (a PEM encoded key include base64 encoded informations and starts with `—–BEGIN RSA PRIVATE KEY—–`.). The public key associated with that private key must be published in a DNS TXT record for entry `<selector>._domainkey.<domain>` where `<selector>` is [dkim_selector](/gpldoc/man/sympa.conf.5.html#dkim_selector) and `<domain>` is [dkim_signer_domain](/gpldoc/man/sympa.conf.5.html#dkim_signer_domain). The signer domain should be the domain of the list ; this is the default, don't change it unless you have strong reason for it.

example with selector = 'lists' and domain 'sympa.org':

``` code
  lists._domainkey.sympa.org. IN TXT "v=DKIM1; g=*; k=rsa; t=y; p=MDB34............DB"
```

In order to generate the public and private keys, you may use `openssl` or `dkim-genkey` included in [milter_dkim software](http://sourceforge.net/projects/dkim-milter/). There are also online tools to generate them, but those services will generate the private key for you (is it still a private key?).

  * http://www.socketlabs.com/services/dkwiz
  * http://www.port25.com/support/support_dkwz.php

Summary of DKIM parameters
--------------------------

| parameter name ([``sympa.conf``](../layout.md#config) or ``robot.conf`` context) | default | overwritten by (list configuration) |
|---|---|---|
| [dkim_feature](/gpldoc/man/sympa.conf.5.html#dkim_feature) | `off` | not pertinent |
| [dkim_add_signature_to](/gpldoc/man/sympa.conf.5.html#dkim_add_signature_to) | `list,robot` | not pertinent |
| [dkim_signature_apply_on](/gpldoc/man/sympa.conf.5.html#dkim_signature_apply_on) | `md5_authenticated_messages,` `smime_authenticated_messages,` `dkim_authenticated_messages,` `editor_validated_messages` | `dkim_signature_apply_on` |
| [dkim_private_key_path](/gpldoc/man/sympa.conf.5.html#dkim_private_key_path) | | dkim > `key_path` |
| [dkim_signer_domain](/gpldoc/man/sympa.conf.5.html#dkim_signer_domain) | the robot domain | dkim > `signer_domain` |
| [dkim_selector](/gpldoc/man/sympa.conf.5.html#dkim_selector) | no default | dkim > `selector` |
| [dkim_signer_identity](/gpldoc/man/sympa.conf.5.html#dkim_signer_identity) | none for robot's messages, _`listname`_`-request@robot` for lists | dkim > `identity_domain` |
| ~~[dkim_header_list](/gpldoc/man/sympa.conf.5.html#dkim_header_list)~~ | as recommended in RFC 6376 | Not yet implemented |

<sup><a href="#fnt__1" id="fn__1" class="fn_bot">1)</a></sup> The private key can't be encrypted with a passphase

Incoming messages ARC
---------------------

Incoming messages will be checked for ARC seals automatically if the [arc_feature](/gpldoc/man/sympa.conf.5.html#arc_feature) is enabled.  The [arc_srvid](/gpldoc/man/sympa.conf.5.html#arc_srvid) configuration parameter must be set to the srvid in the local MTA's `Authentication-Results:` headers if the srvid is not the same as the ARC signer domain.  There is no parameter to control which messages to check because the software automatically checks as needed.

Outgoing messsages ARC
----------------------

Outgoing forwarded messages will have ARC seals added if the [arc_feature](/gpldoc/man/sympa.conf.5.html#arc_feature) is enabled.  ARC uses the same signatures and keys as DKIM, and DKIM is a prerequisite for ARC, so if your ARC seals use the same signer domain and selector as DKIM signatures, as they usually do, they need no further configuration.  If you want to use a different domain or selector which uses a different private key, they can be set in the same way as the DKIM domain, selector, and key. See the parameters below.

Summary of ARC parameters
-------------------------

| parameter name ([``sympa.conf``](../layout.md#config) or ``robot.conf`` context) | default | overwritten by (list configuration) |
|---|---|---|
| [arc_feature](/gpldoc/man/sympa.conf.5.html#dkim_feature) | `off` | not pertinent |
| [arc_srvid](/gpldoc/man/sympa.conf.5.html#arc_srvid) | arc_signer_domain | not pertinent |
| [arc_signer_domain](/gpldoc/man/sympa.conf.5.html#arc_signer_domain) | dkim_signer_domain | dkim > `arc_signer_domain` |
| [arc_selector](/gpldoc/man/sympa.conf.5.html#arc_selector) | dkim_selector | dkim > `arc_selector` |
| [arc_private_key_path](/gpldoc/man/sympa.conf.5.html#arc_private_key_path) | dkim_private_key_path | dkim > `arc_private_key_path` |
