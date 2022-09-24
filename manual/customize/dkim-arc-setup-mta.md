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
results are added as the `Authentication-Results:` header field (AR).

Note that the target of this setting is the _front MTA_
which accepts messages bound for the lists from the Internet.
It is the MTA in your mailing list server,
if that server is directly connected to the Internet,
but otherwise it is in the different mail server(s).

Adittionally, in this chapter you have to choose (or confirm) the
"Authentication Service Identifier" (**authserv-id**) for your mail system.
It will be used later when you configure ARC sealing on Sympa.
It is preferable to be the FQDN of the MTA's host name or
the mail domain name of the service.

Instruction by MTAs
-------------------

  * [Exim](dkim-arc-setup-mta-exim4.md)

  * Using Milter (mail filter for Postfix and Sendmail):

      - [OpenDMARC](dkim-arc-setup-mta-opendmarc.md)

      - [Rspamd](dkim-arc-setup-mta-rspamd.md) ---
        maybe also applicable to OpenSMTPD.

      - authentication_milter ---
        TBD: See the
        [documentation](https://metacpan.org/dist/Mail-Milter-Authentication)
        to get the hang of setup.

  * Using amavisd-new ---
    This only supports DKIM validation among the features we want,
    but it may be useful if other options cannot be taken.
    See the [documentation](https://www.ijs.si/software/amavisd/) for
    details.

Tests
-----

  * Confirm that `Authentication-Results:` header field (AR) with expected
    authserv-id is added by the MTA towards the top of incoming message,
    for example:
    ``` code
    Received: (Information added by MTA...)
    Authetication-Results: mx.example.org;
            dkim=pass ...;
            spf=pass ...;
            dmarc=none
    (The other header fields and body follow...)
    ...
    ```

    Note:
    To prevent forgery, existing AR with the same authserv-id in incoming
    messages would like to be removed in advance.
    If they were not removed, please check the software documentation to
    find out how to do it.

----

After you finished setting up MTA, proceed to
[setup Sympa](dkim-arc-setup-sympa.md).

