---
title: 'DKIM and ARC features for Sympa'
up: ../customize.md#sympa-services-optional-features
redirect_from:
  - dkim.html
---

DKIM and ARC features for Sympa
===============================

  * DKIM feature has been introduced in Sympa version **6.1**.
  * ARC feature has been introduced in Sympa version **6.2.38**.

DKIM is a cryptographic signature method designed to prevent phishing. As
postmaster or listmaster you should consider

 1. checking the DKIM status of each incoming message
 2. signing all or a subset of outgoing messages

ARC is intended to fix the problems introduced by [DMARC](dmarc-protection.md) by adding signature "seals" that show the chain of servers that processed a message.  Once ARC is more widely implemented, DMARC workarounds shouldn't be needed.  Once you have DKIM set up, adding ARC seals is straightforward.

Processing of DKIM status of incoming messages is done by the MTA (Mail transfer agent) that delivers emails to the Sympa server Sympa domain (in order to reject some messages). This topic is not addressed in the chapter of the documentation.

The mailing list server can take advantage of incoming DKIM signatures in order to measure the trust of the message while evaluating message workflow. This is based on [scenario mechanism](basics-scenarios.md). An authentication level named `dkim` can be used within scenario rules to check that an incoming message has a valid DKIM signature (dkim signature status = pass).

In addition, you must consider signature of outgoing messages. Should messages broadcasted by Sympa to list subscribers be signed by your organization? Should all of them be signed? Should a subset of trusted messages be signed? Should service messages (automatic answer, welcome messages etc) be signed ?  In most cases Sympa should sign all the mail it sends to get the most benefit from DKIM.

ARC seals only make sense on mail forwarded by Sympa, that is, individual messages sent through to mailing lists. The ARC feature will only add seals to those messages.  ARC checks the ARC seals, if any, on incoming messages, 

Prerequisites
-------------

### Prerequisites for DKIM

DKIM features in Sympa are based on the **[Mail::DKIM](https://metacpan.org/release/Mail-DKIM)** cpan module ; you should install it first. Check [the documentation related to cpan modules installation](../install/install-dependent-modules.md).

To enable Sympa's DKIM signing feature, you have to publish public key for
signature verification as DNS resource record.  That is, you have to
have the permission to add DNS RR.

### Prerequisites for ARC

ARC requires Mail::DKIM version 0.55 or later which includes ARC support.

Like DKIM, ARC requires the keys for signing, but the DKIM key may be
diverted for it.

It also requires that the MTA that delivers emails to Sympa adds an
`Authentication-Results:` header that shows how a message was (or wasn't)
authenticated as it arrived (See succeeding chapters to know details).

Configuration instruction
-------------------------

  1. [Setup key pair for DKIM / ARC](dkim-arc-setup-keys.md)
     (for DKIM signing and/or ARC)

  2. [Setup MTA](dkim-arc-setup-mta.md) (for ARC)

  3. [Setup Sympa](dkim-arc-setup-sympa.md)

