---
title: 'DKIM and ARC: Setup MTA'
up: ../dkim-arc.md
prev: setup-keys.md
next: setup-sympa.md
---

DKIM and ARC: Setup MTA
=======================

  * Steps described in this chapter are required to support ARC.
    If you are not planning to support it,
    [skip this chapter](setup-sympa.md).

In this chapter, how to set up MTA so that the feature of sender domain
validation with SPF, DKIM, DMARC and possiblly ARC are enabled and their
results are added as the `Authentication-Results:` header field (AR).

> **Note**
>   * Since Sympa 6.2.71b.1 and later, Sympa is able to verify DKIM
>     signatures and ARC seals in incoming messages by itself and add the
>     results as AR, so it can work anyway even without the settings
>     described in this chapter.
>     However, those settings are strongly recommended to provide
>     sufficient authentication results.
>
>     With Sympa 6.2.70 or earlier, the settings are required.

Requirements
------------

  * Working with the front MTA

    Note that the target of this setting is the _front MTA_
    which accepts messages bound for the lists from the Internet.
    It is the MTA in your mailing list server,
    if that server is directly connected to the Internet.
    But otherwise, it is in the different mail server(s).

  * Authentication Service Identifier (authserv-id)

    In this chapter you have to choose (or confirm) the
    "Authentication Service Identifier" (**authserv-id**) for your mail
    system.
    It will be used later when you configure ARC sealing on Sympa.
    It is preferable to be the FQDN of the MTA's host name or
    the mail domain name of the service.

    In this document `mx.example.org` is used for example.

Instruction by MTAs
-------------------

> **Note**
>   * Some of the software described in below have DKIM signing and/or
>     ARC sealing capability for outgoing messages, but we may not
>     configure them for now:
>     Sympa is responsible for those features.

  * [Exim](setup-mta-exim4.md)

  * Using Milter (mail filter for Postfix and Sendmail):

      - [Authentication Milter](setup-mta-auththentication_milter.md)

      - [OpenDMARC](setup-mta-opendmarc.md)

      - [Rspamd](setup-mta-rspamd.md)

  * Rspamd is maybe also applicable to OpenSMTPD.
    TBD.


Tests
-----

  * Confirm that `Authentication-Results:` header field (AR) with expected
    authserv-id is added by the MTA towards the top of incoming message,
    for example:
    ``` code
    Received: (Information added by MTA...)
    Authentication-Results: mx.example.org;
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
[setup Sympa](setup-sympa.md).

