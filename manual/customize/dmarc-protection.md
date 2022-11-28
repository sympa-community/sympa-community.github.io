---
title: 'DMARC protection'
up: ../customize.md#sympa-services-optional-features
---

DMARC protection
================

  * DMARC protection feature was introduced on Sympa 6.1.22 and 6.2

[DMARC](http://www.dmarc.org/) stands for
"Domain-based Message Authentication, Reporting & Conformance".
It is anti-spam and phishing measure supported by major mail providers
such as yahoo or hotmail.

This is a quick update for now. If you want to learn more about DMARC and what awful things its rough application did to mailing lists manager, please read [Related posts](#related-posts) below.

ARC ([RFC 8617](https://tools.ietf.org/html/rfc8617.html)) is intended to
fix the problems introduced by DMARC by adding signatures (they are called
"seals") that show the chain of servers that processed a message.
Once ARC is more widely implemented, workarounds by
[DMARC protection](dmarc-protection.md) shouldn't be needed.
So [setting up ARC](dkim-arc.md) on your mailing list server is recommended,
so that impact by DMARC protection would be reduced
(Once you have DKIM set up, adding ARC seals is straightforward).

Background
----------

To make a long story short: yahoo, then aol and probably others set the "`p=reject`" tag in their DMARC DNS record. This means: _reject anything that doesn't match my security policies_. OK. But Sympa and most mailing lists managers would break this policy, simply by changing the mail subject or the `Return-Path` because:

  - yahoo requires DKIM valid - yahoo domain-signed - signature for any mails from its domain

  - Sympa would break this signature by changing the `Return-Path` or `Subject` - which are parts of the yahoo signature

The messages to yahoo would bounce "for policy reason" but worse: messages sent to receipients whose domain applies yahoo DMARC policy would also bounce. Examples of such domains are gmail, hotmail, etc.

How it works
------------

DMARC protection will have the following effect:
The mails from domains whose pollicy is to reject any mail not respecting
its DMARC policy will be processed this way:

  1. Their DKIM and DomainKey signatures will be removed.

  2. The address in `From:` field will be changed to the list address
     (or anything specified in list config) so that the message will pass
     validation by DMARC.

     For convenience of recipients, information of original sender are
     embedded in the display name and/or comment in the `From:` field.

Previous values of the `From:` header field and the DKIM signature are saved
in an `X-Original-From:` and `X-Original-DKIM-Signature:` header fields
for later inspection.

How to setup
------------

  1. Install [Net-DNS](https://metacpan.org/release/Net-DNS) Perl module, if
     it have not been installed.

  2. Set the `dmarc_protection.mode` to the value you wish. For a quick
     correction on the most restrictive DMARC records, just add the
     following line to your sympa.conf:
     ``` code
     dmarc_protection.mode dmarc_reject
     ```
     > **Note**
     >
     >   * On Sympa 6.2.56 or earlier, the name of the parameter above should
     >     be `dmarc_protection_mode`.

You can get further customization of how to deal with DMARC by using the
other
[dmarc_protection](/gpldoc/man/sympa_config.5.html#dmarc_protection)
parameters.

Related posts
-------------

  - [A post by John Levine exposing the problem](https://jl.ly/Email/yahoobomb.html),

  - [The crucial discussion about it on sympa-users](https://lists.sympa.community/msg/en/2014-04/Wj1SFwl0SwSXPkdMg0T-wQ).

Acknowledgement
---------------

You can all thank **Steve Shipway** for his deep understanding of the problem and the patch he provided to solve the issue.
