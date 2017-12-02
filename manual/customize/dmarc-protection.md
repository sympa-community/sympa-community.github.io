DMARC protection
================

  * DMARC protection feature was introduced on Sympa 6.1.22 and 6.2

[DMARC](http://www.dmarc.org/) stands for "Domain-based Message Authentication, Reporting & Conformance". It is spam & phishing protection setup by big mail providers such as yahoo or hotmail.

This is a quick update for now. If you want to learn more about DMARC and what awful things its rough application did to mailing lists manager, please read [Related posts](#related-posts) below.

To make a long story short: yahoo, then aol and probably others set the "`p=reject`" tag in their DMARC DNS record. This means: _reject anything that doesn't match my security policies_. OK. But Sympa and most mailing lists managers would break this policy, simply by changing the mail subject or the `Return-Path` because:

  - yahoo requires DKIM valid - yahoo domain-signed - signature for any mails from its domain

  - Sympa would break this signature by changing the `Return-Path` or `Subject` - which are parts of the yahoo signature

The messages to yahoo would bounce "for policy reason" but worse: messages sent to receipients whose domain applies yahoo DMARC policy would also bounce. Examples of such domains are gmail, hotmail, etc.

To fix it:

  - Set the `dmarc_protection_mode` to the value you wish. For a quick correction on the most restrictive DMARc records, just add the following line to your sympa.conf:

    ``` code
    dmarc_protection_mode dmarc_reject
    ```

This will have the following effect: mails from domains whose pollicy is to reject any mail not respecting its DMARC policy will be processed this way:

  1. their DKIM and DomainKey signatures will be removed

  2. the `From:` field will be changed to a value you will set in the `dmarc_protection_other_email` parameter. You can define a value for this email or leave it blank. If so, the list email address will be set instead of the original sender address.

  3. previous value of the `From:` header field is saved in an `X-Original-From:` header field for later inspection

  4. previous value of the DKIM signature is saved in `X-Original-DKIM-Signature:` field for later inspection.

You can get further customization of how to deal with DMARC by using the different Sympa [dmarc_protection*](../man/sympa.conf.5.md#dmarc-protection) parameters.

Related posts
-------------

  - [A post by John Levine exposing the problem](https://jl.ly/Email/yahoobomb.html),

  - [The crucial discussion about it on sympa-users](https://listes.renater.fr/sympa/arc/sympa-users/2014-04/msg00026.html).

Acknowledgement
---------------

You can all thank **Steve Shipway** for his deep understanding of the problem and the patch he provided to solve the issue.
