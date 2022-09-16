---
title: 'DKIM and ARC: Setup MTA'
up: dkim-arc.md
prev: dkim-arc-setup-keys.md
next: dkim-arc-setup-sympa.md
---

DKIM and ARC: Setup MTA
=======================

  * Steps described in this chapter are required to support ARC.
    If you are not planning to support it,
    [skip this chapter](dkim-arc-setup-sympa.md).

In this chapter, how to set up MTA so that the feature of sender domain
validation with SPF, DKIM, DMARC and possiblly ARC are enabled and their
results are added as the `Authentication-Results:` header field.

In this procedure you have to choose (or confirm) the
"Authentication Service Identifier" (**authserv-id**) for your mail system.
It will be used later when you configure ARC sealing on Sympa.

Instruction by MTAs
-------------------

  * ~[Exim](dkim-arc-setup-mta-exim.md)~

    (Instruction is work in progress)

  * Using Milter programs (for Postfix and Sendmail):

      - [OpenDMARC](dkim-arc-setup-mta-opendmarc.md)

      - [Rspamd](dkim-arc-setup-mta-rspamd.md) ---
        maybe also applicable to OpenSMTPD

      - Authentication Milter

        (TBD: See the description in the
        [source repository](https://github.com/fastmail/authentication_milter)
        to get the hang of setup)

  * Using [amavisd-new](https://www.ijs.si/software/amavisd/)

    This only supports DKIM among the features we want,
    but it may be useful if other options cannot be taken.
    See the [documentation](https://www.ijs.si/software/amavisd/) for
    details.

After you finished setting up MTA, proceed to
[setup Sympa](dkim-arc-setup-sympa.md).

