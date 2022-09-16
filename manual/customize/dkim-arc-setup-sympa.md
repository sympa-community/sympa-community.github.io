---
title: 'DKIM and ARC: Setup Sympa'
up: dkim-arc.md
next: dkim-arc.md#tests
---

DKIM and ARC: Setup Sympa
=========================

You may enable one or more of three features:
DKIM signature verification for incoming messages,
DKIM signing for outgoing messages
and ARC verification / sealing.

Additionally, a workaround is recommended for sites with unsatisfactory
support for ARC.

DKIM signature verification for incoming messages
-------------------------------------------------

To make Sympa check the DKIM signature of incoming messages, you need to set the [dkim_feature](/gpldoc/man/sympa_config.5.html#dkim_feature) configuration parameter to `on`. Before doing that you must first update your customized scenario to introduce `dkim` authentication method, **otherwise Sympa may reject messages because they include a valid DKIM signature !**. All default scenarios starting at version 6.1 already include rules for DKIM, both for command and lists messages.

**Possible changes in scenarios**

Turning on the [dkim_feature](/gpldoc/man/sympa_config.5.html#dkim_feature) configuration parameter will provide a new authentication level to the scenario engine. Scenario evaluation for incoming messages with a valid DKIM signature (but no S/MIME signature) will be evaluated with authentication method `dkim`. So rules that use authentication method `smtp` will not match.

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
<!--
[Content of AR field has to be parsed.  Matching by regexp does not make sense]

If the front MTA adds the
[`Authentication-Results`](https://tools.ietf.org/html/rfc8601)
header field, Sympa can of course check this SMTP header field using
standard match() and equal() scenario conditions.
-->

DKIM signing for outgoing messages
----------------------------------

You may want to make Sympa sign outgoing messages.  Almost every aspects
of DKIM signature behavior can be customized via Sympa configuration
parameters.  Please check the
[DKIM parameters section](/gpldoc/man/sympa_config.5.html#dkim-dmarc-arc)
for further details.  Note that each parameter can also be set for a given
virtual domain; and most of them are available as list parameters.

### Which messages should be signed

In order to configure Sympa for signing outgoing messages, you have to decide **which messages Sympa should sign** . This should be decided for four kind of messages:

  - Services messages : these are all messages sent by Sympa itself : welcome messages, answers to mail commands, various notification such as *remind message* and digest messages;

  - List messages : messages distributed to list members (where the initial `From:` header is preserved). These messages will fall info one of these subcategories:

      - authenticated messages (using S/MIME signature, challenge or password);

      - received with a valid DKIM signature;

      - validated by one of the list moderators;

      - other messages.

This behavior is controlled by [dkim_add_signature_to](/gpldoc/man/sympa_config.5.html#dkim_add_signature_to) and [dkim_signature_apply_on](/gpldoc/man/sympa_config.5.html#dkim_signature_apply_on) parameters.

In most cases Sympa should sign **all** outgoing messages to get the maximum advantage from DKIM.

### Parameters for DKIM signing

Before Sympa is able to DKIM-sign messages, you need to set several related
parameters.  The most important ones are
[dkim_private_key_path](/gpldoc/man/sympa_config.5.html#dkim_private_key_path)
(private key file location) and
[dkim_selector](/gpldoc/man/sympa_config.5.html#dkim_selector).
Other parameters related to [RFC 6376](https://tools.ietf.org/html/rfc6376):
[dkim_signer_domain](/gpldoc/man/sympa_config.5.html#dkim_signer_domain),
[dkim_signer_identity](/gpldoc/man/sympa_config.5.html#dkim_signer_identity)
(for domain) /
[dkim_parameters.signer_identity](/gpldoc/man/sympa_config.5.html#dkim_parameterssigner_identity)
(for lists),
[dkim_header_list](/gpldoc/man/sympa_config.5.html#dkim_header_list)
(not yet implemented).

----
Notes:

  * `dkim_signer_identity` in `sympa.conf` or `robot.conf` _won't_ be
    overwritten by `dkim_parameters > signer_identity`,
    in other words the former is not treated as the default for the latter.

  * As of Sympa 6.2.72, some parameters in `sympa.conf` and `robot.conf`
    will be renamed:

      - `dkim_private_key_path` to `dkim_parameters.private_key_path`
      - `dkim_signer_domain` to `dkim_parameters.signer_domain`
      - `dkim_selector` to `dkim_parameters.selector`

----

ARC
---

### ARC verification for incoming messages

Incoming messages will be checked for ARC seals automatically if the
[arc_feature](/gpldoc/man/sympa_config.5.html#arc_feature) is enabled.
The [arc_srvid](/gpldoc/man/sympa_config.5.html#arc_srvid) configuration
parameter must be set to the **authserv-id** you chose during
[setup of front MTA](dkim-arc-setup-mta.md), if that authserv-id is not the
same as the ARC signer domain.  There is no parameter to control which
messages to check because the software automatically checks as needed.

### ARC sealing for outgoing messsages

Outgoing forwarded messages will have ARC seals added if the [arc_feature](/gpldoc/man/sympa_config.5.html#arc_feature) is enabled.  ARC uses the same signatures and keys as DKIM, and DKIM is a prerequisite for ARC, so if your ARC seals use the same signer domain and selector as DKIM signatures, as they usually do, they need no further configuration.  If you want to use a different domain or selector which uses a different private key, they can be set in the same way as the DKIM domain, selector, and key. See the parameters below.

----
Note:

  * On Sympa 6.2.72 or later, ARC seals will also be added to the
    messages forwarded through the addresses of listmaster, list owner
    and list moderators.
    On previous versions, ARC seals are added to only the messages sent
    through the list posting addresses.

  * As of Sympa 6.2.72, some parameters in `sympa.conf` and `robot.conf`
    will be renamed:

      - `arc_signer_domain` to `arc_parameters.signer_domain`
      - `dkim_selector` to `arc_parameters.selector`
      - `arc_private_key_path` to `arc_parameters.private_key_path`

----

Remedy for ARC-unaware recipients
---------------------------------

In recent years, the number of DMARC-aware sites has been increasing,
but support for ARC is still insufficient.  For this reason, some
recipient sites reject message delivery based solely on the result of DMARC
verification without ARC verification.

As a remedy, it is recommended to enalbe
[DMARC protection](dmarc-protection.md) for domains that have adopted the
`reject` policy.  To do it, add following line to `sympa.conf` or
`robot.conf`:

``` code
dmarc_protection.mode dmarc_reject
```
----
Note:

  * With Sympa 6.2.56 or earlier, the setting above has to be:
    ``` code
    dmarc_protection_mode dmarc_reject
    ```
----

Summary of parameters
---------------------

| parameter name ([``sympa.conf``](../layout.md#config) or ``robot.conf`` context) | default | overwritten by (list configuration) |
|---|---|---|
| [dkim_feature](/gpldoc/man/sympa_config.5.html#dkim_feature) | `off` | not pertinent |
| [dkim_add_signature_to](/gpldoc/man/sympa_config.5.html#dkim_add_signature_to) | `list,robot` | not pertinent |
| [dkim_signature_apply_on](/gpldoc/man/sympa_config.5.html#dkim_signature_apply_on) | `md5_authenticated_messages,` `smime_authenticated_messages,` `dkim_authenticated_messages,` `editor_validated_messages` | `dkim_signature_apply_on` |
| [dkim_private_key_path](/gpldoc/man/sympa_config.5.html#dkim_private_key_path) | | `dkim_parameters` > `private_key_path` |
| [dkim_signer_domain](/gpldoc/man/sympa_config.5.html#dkim_signer_domain) | the mail domain | `dkim_parameters` > `signer_domain` |
| [dkim_selector](/gpldoc/man/sympa_config.5.html#dkim_selector) | no default | `dkim_parameters` > `selector` |
| [dkim_signer_identity](/gpldoc/man/sympa_config.5.html#dkim_signer_identity)<sup>*</sup> | (for messages in domain context) none | none |
| [dkim_parameters > signer_identity](/gpldoc/man/sympa_config.5.html#dkim_parameterssigner_identity)<sup>*</sup> | (for lists) _`listname`_`-request@domain` | - |
| ~~[dkim_header_list](/gpldoc/man/sympa_config.5.html#dkim_header_list)~~ | as recommended in RFC 6376 | Not yet implemented |
| [arc_feature](/gpldoc/man/sympa_config.5.html#arc_feature) | `off` | not pertinent |
| [arc_srvid](/gpldoc/man/sympa_config.5.html#arc_srvid) | arc_signer_domain | not pertinent |
| [arc_signer_domain](/gpldoc/man/sympa_config.5.html#arc_signer_domain) | dkim_signer_domain | `arc_parameters` > `arc_signer_domain` |
| [arc_selector](/gpldoc/man/sympa_config.5.html#arc_selector) | dkim_selector | `arc_parameters` > `arc_selector` |
| [arc_private_key_path](/gpldoc/man/sympa_config.5.html#arc_private_key_path) | dkim_private_key_path | `arc_parameters` > `arc_private_key_path` |

Tests
-----

  * If you enabled DKIM signing, confirm that `DKIM-Signature` field with
    expected signing domain (the same as mail domain) is added
    towards the beginning of the message sent through Sympa.

  * If you enabled ARC, confirm that `ARC-Seal:`, `ARC-Message-Signature:`
    and `ARC-Authentication-Results:` fields with expected authserv-id and
    signing domain are added towards the beginning of message sent
    through Sympa.

----
Note:

  * On Sympa 6.2.72 or later, if ARC feature is enabled,
    DKIM signature will be added to outgoing messages as necessity,
    regardless to availability of the DKIM feature.

    Conversely on earlier version of Sympa, ARC sealing is possible only
    when DKIM signature is allowed by
    [`dkim_signature_apply_on`](/gpldoc/man/sympa_config.5.html#dkim_signature_apply_on)
    parameter.

----

