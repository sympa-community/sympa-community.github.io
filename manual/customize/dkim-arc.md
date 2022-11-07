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

DKIM ([RFC 6376](https://tools.ietf.org/html/rfc6376.html)) is a
cryptographic signature method designed to prevent phishing.
As postmaster or listmaster you should consider:

 1. checking the DKIM status of each incoming message
 2. signing all or a subset of outgoing messages

ARC ([RFC 8617](https://tools.ietf.org/html/rfc8617.html)) is intended to
fix the problems introduced by DMARC by adding signatures (they are called
"seals") that show the chain of servers that processed a message.
Once ARC is more widely implemented, workarounds by
[DMARC protection](dmarc-protection.md) shouldn't be needed.
So [setting up ARC](dkim-arc.md) on your mailing list server is recommended,
so that impact by DMARC protection would be reduced
(Once you have DKIM set up, adding ARC seals is straightforward).

Processing of DKIM status of incoming messages is done by the MTA (Mail transfer agent) that delivers emails to the Sympa server Sympa domain (in order to reject some messages). This topic is not addressed in the chapter of the documentation.

You must consider signature of outgoing messages.
Should messages broadcasted by Sympa to list subscribers be signed by your
organization?  Should all of them be signed? Should a subset of trusted
messages be signed?  Should service messages (automatic answer, welcome
messages etc) be signed?
In most cases Sympa should sign all the mail it sends to get the most
benefit from DKIM.

ARC seals only make sense on mail forwarded by Sympa, that is, individual messages sent through to mailing lists. The ARC feature will only add seals to those messages.  ARC checks the ARC seals, if any, on incoming messages, 

----
Note:

  * On Sympa 6.2.72 or later, ARC seals will also be added to the
    messages forwarded through the addresses of listmaster, list owner
    and list moderators.
    On previous versions, ARC seals are added to only the messages sent
    through the list posting addresses.

  * On Sympa 6.1 to 6.2.70, before enabling DKIM feature you may
    have to update your customized scenario to introduce `dkim`
    authentication method.  See
    "[The `dkim` authentication method for scenarios](basics-scenarios-dkim.md)".

----

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

It also requires that the front MTA that delivers emails to Sympa adds an
`Authentication-Results:` header field that shows how a message was
(or wasn't) authenticated as it arrived.
You will know how to do it in succeeding chapters.

Configuration instruction
-------------------------

  1. [Setup key pair for DKIM / ARC](dkim-arc-setup-keys.md)
     (for DKIM signing and/or ARC)

  2. [Setup MTA](dkim-arc-setup-mta.md) (for ARC)

  3. [Setup Sympa](dkim-arc-setup-sympa.md)

