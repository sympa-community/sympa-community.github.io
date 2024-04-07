---
title: 'DKIM and ARC: Setup Sympa'
up: ../dkim-arc.md
next: ../dkim-arc.md#tests
---

DKIM and ARC: Setup Sympa
=========================

You may enable one or more of three features:
DKIM signature verification for incoming messages,
DKIM signing for outgoing messages
and ARC verification / sealing.

Additionally, a workaround is recommended for sites with unsatisfactory
support for ARC.

> **Note**
>
>   * On Sympa 6.1 to 6.2.70, before enabling DKIM feature you may
>     have to update your customized scenario to introduce `dkim`
>     authentication method.  See
>     "[The `dkim` authentication method for scenarios](../../customize/basics-scenarios-dkim.md)".

<!--
[Content of AR field has to be parsed.  Matching by regexp does not make sense]

If the front MTA adds the
[`Authentication-Results`](https://tools.ietf.org/html/rfc8601)
header field, Sympa can of course check this SMTP header field using
standard match() and equal() scenario conditions.
-->

DKIM signing for outgoing messages
----------------------------------

  * See also "[Summary of parameters](#summary-of-parameters)" below.

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
[`dkim_parameters.private_key_path`](/gpldoc/man/sympa_config.5.html#dkim_parametersprivate_key_path)
(private key file location) and
[`dkim_parameters.selector`](/gpldoc/man/sympa_config.5.html#dkim_parametersselector).
Other parameters related to [RFC 6376](https://tools.ietf.org/html/rfc6376):
[`dkim_parameters.signer_domain`](/gpldoc/man/sympa_config.5.html#dkim_parameterssigner_domain),
[dkim_signer_identity](/gpldoc/man/sympa_config.5.html#dkim_signer_identity)
(for domain) /
[dkim_parameters.signer_identity](/gpldoc/man/sympa_config.5.html#dkim_parameterssigner_identity)
(for lists),
[dkim_header_list](/gpldoc/man/sympa_config.5.html#dkim_header_list)
(not yet implemented).

If you are using Sympa prior to 6.2.72, see also the note below.

> **Note**
>
>   * `dkim_signer_identity` in `sympa.conf` or `robot.conf` _won't_ be
>     overwritten by `dkim_parameters > signer_identity`,
>     in other words the former is not treated as the default for the latter.
>
>   * On Sympa prior to 6.2.72, some parameters in `sympa.conf` and
>     `robot.conf` were not the same as those in the list `config` file:
>
>       - "`dkim_private_key_path`" for `dkim_parameters.private_key_path`
>       - "`dkim_selector`" for `dkim_parameters.selector`
>       - "`dkim_signer_domain`" for `dkim_parameters.signer_domain`

ARC
---

  * See also "[Summary of parameters](#summary-of-parameters)" below.

### ARC verification for incoming messages

Incoming messages will be checked for ARC seals automatically if the
[arc_feature](/gpldoc/man/sympa_config.5.html#arc_feature) is enabled.
The [arc_srvid](/gpldoc/man/sympa_config.5.html#arc_srvid) configuration
parameter must be set to the **authserv-id** you chose during
[setup of front MTA](setup-mta.md), if that authserv-id is not the
same as the ARC signer domain.  There is no parameter to control which
messages to check because the software automatically checks as needed.

### ARC sealing for outgoing messsages

Outgoing forwarded messages will have ARC seals added if the
[arc_feature](/gpldoc/man/sympa_config.5.html#arc_feature)
is enabled.  ARC feature can use the same keys as DKIM, and DKIM is a
prerequisite for ARC, so if your ARC seals use the same signer domain and
selector as DKIM signatures, as they usually do, they may need no further
configuration.  If you want to use a different domain or selector which uses
a different key pair, they can be set in the same way as the DKIM domain,
selector, and key. See the parameters below.

> **Note**
>
>   * On Sympa prior to 6.2.72, ARC seals were added to only the messages sent
>     through the list posting addresses.
>     On Sympa 6.2.72 or later, ARC seals are also added to the
>     messages forwarded through the addresses of listmaster, list owners
>     and list moderators.
>
>   * On Sympa prior to 6.2.72, some parameters in `sympa.conf` and
>     `robot.conf` were not the same as those in the list `config` file:
>
>       - "`arc_private_key_path`" for `arc_parameters.private_key_path`
>       - "`dkim_selector`" for `arc_parameters.selector`
>       - "`arc_signer_domain`" for `arc_parameters.signer_domain`

Remedy for ARC-unaware recipients
---------------------------------

In recent years, the number of DMARC-aware sites has been increasing,
but support for ARC is still insufficient.  For this reason, some
recipient sites reject message delivery based solely on the result of DMARC
verification without ARC verification.

As a remedy, it is recommended to enable
[DMARC protection](../../customize/dmarc-protection.md) for domains that have adopted the
`reject` policy.  To do it, add following line to `sympa.conf` or
`robot.conf`:

``` code
dmarc_protection.mode dmarc_reject
```

If you are using Sympa prior to 6.2.56, see also the note below.

> **Note**
>
>   * On Sympa prior to 6.2.56, the parameter above in `sympa.conf` and
>     `robot.conf` was not the same as those in the list `config` file:
>
>     ``` code
>     dmarc_protection_mode dmarc_reject
>     ```

Summary of parameters
---------------------

### 6.2.72 or later

| parameter name ([``sympa.conf``](../../layout.md#config) or ``robot.conf`` context) | default | overwritten by (list configuration) |
|---|---|---|
| [`dkim_feature`](/gpldoc/man/sympa_config.5.html#dkim_feature) | `off` | not pertinent |
| [`dkim_add_signature_to`](/gpldoc/man/sympa_config.5.html#dkim_add_signature_to) | `list,robot` | not pertinent |
| [`dkim_signature_apply_on`](/gpldoc/man/sympa_config.5.html#dkim_signature_apply_on) | `md5_authenticated_messages,` `smime_authenticated_messages,` `dkim_authenticated_messages,` `editor_validated_messages` | [`dkim_signature_apply_on`](/gpldoc/man/sympa_config.5.html#dkim_signature_apply_on) |
| [`dkim_parameters.private_key_path`](/gpldoc/man/sympa_config.5.html#dkim_parametersprivate_key_path) | `arc_parameters.private_key_path` if set | [`dkim_parameters` > `private_key_path`](/gpldoc/man/sympa_config.5.html#dkim_parametersprivate_key_path) |
| [`dkim_parameters.signer_domain`](/gpldoc/man/sympa_config.5.html#dkim_parameterssigner_domain) | `arc_parameters.signer_domain` if set, otherwise the mail domain | [`dkim_parameters` > `signer_domain`](/gpldoc/man/sympa_config.5.html#dkim_parameterssigner_domain) |
| [`dkim_parameters.selector`](/gpldoc/man/sympa_config.5.html#dkim_parametersselector) | `arc_parameters.selector` if set | [`dkim_parameters` > `selector`](/gpldoc/man/sympa_config.5.html#dkim_parametersselector) |
| [`dkim_signer_identity`](/gpldoc/man/sympa_config.5.html#dkim_signer_identity)<sup>(\*)</sup> | (for messages in domain context) none | none |
| --- | (for lists) _`listname`_`-request@domain` | [`dkim_parameters` > `signer_identity`](/gpldoc/man/sympa_config.5.html#dkim_parameterssigner_identity)<sup>(\*)</sup> |
| [`arc_feature`](/gpldoc/man/sympa_config.5.html#arc_feature) | `off` | not pertinent |
| [`arc_srvid`](/gpldoc/man/sympa_config.5.html#arc_srvid) | value of `arc_parameters.signer_domain`: see below | not pertinent |
| [`arc_parameters.signer_domain`](/gpldoc/man/sympa_config.5.html#arc_parameterssigner_domain) | `dkim_parameters.signer_domain` if set, otherwise the mail domain | [`arc_parameters` > `signer_domain`](/gpldoc/man/sympa_config.5.html#arc_parameterssigner_domain) |
| [`arc_parameters.selector`](/gpldoc/man/sympa_config.5.html#arc_parametersselector) | `dkim_parameters.selector` if set | [`arc_parameters` > `selector`](/gpldoc/man/sympa_config.5.html#arc_parametersselector) |
| [`arc_parameters.private_key_path`](/gpldoc/man/sympa_config.5.html#arc_parametersprivate_key_path) | `dkim_parameters.private_key_path` if set | [`arc_parameters` > `private_key_path`](/gpldoc/man/sympa_config.5.html#arc_parametersprivate_key_path) |
| [`dmarc_protection.mode`](/gpldoc/man/sympa_config.5.html#dmarc_protectionmode) | none | [`dmarc_protection` > `mode`](/gpldoc/man/sympa_config.5.html#dmarc_protectionmode) |

### 6.2.58 to 6.2.70

| parameter name ([``sympa.conf``](../../layout.md#config) or ``robot.conf`` context) | default | overwritten by (list configuration) |
|---|---|---|
| [`dkim_feature`](/gpldoc/man/sympa_config.5.html#dkim_feature) | `off` | not pertinent |
| [`dkim_add_signature_to`](/gpldoc/man/sympa_config.5.html#dkim_add_signature_to) | `list,robot` | not pertinent |
| [`dkim_signature_apply_on`](/gpldoc/man/sympa_config.5.html#dkim_signature_apply_on) | `md5_authenticated_messages,` `smime_authenticated_messages,` `dkim_authenticated_messages,` `editor_validated_messages` | [`dkim_signature_apply_on`](/gpldoc/man/sympa_config.5.html#dkim_signature_apply_on) |
| [**`dkim_private_key_path`**](/gpldoc/man/sympa_config.5.html#dkim_private_key_path) | **none** | [`dkim_parameters` > `private_key_path`](/gpldoc/man/sympa_config.5.html#dkim_parametersprivate_key_path) |
| [**`dkim_signer_domain`**](/gpldoc/man/sympa_config.5.html#dkim_signer_domain) | **none**: the mail domain should be set<sup>(\*\*)</sup> | [`dkim_parameters` > `signer_domain`](/gpldoc/man/sympa_config.5.html#dkim_parameterssigner_domain) |
| [**`dkim_selector`**](/gpldoc/man/sympa_config.5.html#dkim_selector) | **none** | [`dkim_parameters` > `selector`](/gpldoc/man/sympa_config.5.html#dkim_parametersselector) |
| [`dkim_signer_identity`](/gpldoc/man/sympa_config.5.html#dkim_signer_identity)<sup>(\*)</sup> | (for messages in domain context) none | none |
| --- | (for lists) _`listname`_`-request@domain` | [`dkim_parameters > signer_identity`](/gpldoc/man/sympa_config.5.html#dkim_parameterssigner_identity)<sup>(\*)</sup> |
| [`arc_feature`](/gpldoc/man/sympa_config.5.html#arc_feature) | `off` | not pertinent |
| [`arc_srvid`](/gpldoc/man/sympa_config.5.html#arc_srvid) | `arc_signer_domain` | not pertinent |
| [**`arc_signer_domain`**](/gpldoc/man/sympa_config.5.html#arc_signer_domain) | `dkim_signer_domain` | **[`arc_parameters` > `arc_signer_domain`](/gpldoc/man/sympa_config.5.html#arc_parametersarc_signer_domain)** |
| [**`arc_selector`**](/gpldoc/man/sympa_config.5.html#arc_selector) | `dkim_selector` | **[`arc_parameters` > `arc_selector`](/gpldoc/man/sympa_config.5.html#arc_parametersarc_selector)** |
| [**`arc_private_key_path`**](/gpldoc/man/sympa_config.5.html#arc_private_key_path) | `dkim_private_key_path` | **[`arc_parameters` > `arc_private_key_path`](/gpldoc/man/sympa_config.5.html#arc_parametersarc_private_key_path)** |
| [`dmarc_protection.mode`](/gpldoc/man/sympa_config.5.html#dmarc_protectionmode) | none | [`dmarc_protection` > `mode`](/gpldoc/man/sympa_config.5.html#dmarc_protectionmode) |

### Up to 6.2.56

| parameter name ([``sympa.conf``](../../layout.md#config) or ``robot.conf`` context) | default | overwritten by (list configuration) |
|---|---|---|
| [`dkim_feature`](/gpldoc/man/sympa_config.5.html#dkim_feature) | `off` | not pertinent |
| [`dkim_add_signature_to`](/gpldoc/man/sympa_config.5.html#dkim_add_signature_to) | `list,robot` | not pertinent |
| [`dkim_signature_apply_on`](/gpldoc/man/sympa_config.5.html#dkim_signature_apply_on) | `md5_authenticated_messages,` `smime_authenticated_messages,` `dkim_authenticated_messages,` `editor_validated_messages` | [`dkim_signature_apply_on`](/gpldoc/man/sympa_config.5.html#dkim_signature_apply_on) |
| [**`dkim_private_key_path`**](/gpldoc/man/sympa_config.5.html#dkim_private_key_path) | none | [`dkim_parameters` > `private_key_path`](/gpldoc/man/sympa_config.5.html#dkim_parametersprivate_key_path) |
| [**`dkim_signer_domain`**](/gpldoc/man/sympa_config.5.html#dkim_signer_domain) | **none**: the mail domain should be set<sup>(\*\*)</sup> | [`dkim_parameters` > `signer_domain`](/gpldoc/man/sympa_config.5.html#dkim_parameterssigner_domain) |
| [**`dkim_selector`**](/gpldoc/man/sympa_config.5.html#dkim_selector) | none | [`dkim_parameters` > `selector`](/gpldoc/man/sympa_config.5.html#dkim_parametersselector) |
| [`dkim_signer_identity`](/gpldoc/man/sympa_config.5.html#dkim_signer_identity)<sup>(\*)</sup> | (for messages in domain context) none | none |
| --- | (for lists) _`listname`_`-request@domain` | [`dkim_parameters` > `signer_identity`](/gpldoc/man/sympa_config.5.html#dkim_parameterssigner_identity)<sup>(\*)</sup> |
| [`arc_feature`](/gpldoc/man/sympa_config.5.html#arc_feature) | `off` | not pertinent |
| [`arc_srvid`](/gpldoc/man/sympa_config.5.html#arc_srvid) | `arc_signer_domain` | not pertinent |
| [**`arc_signer_domain`**](/gpldoc/man/sympa_config.5.html#arc_signer_domain) | **none**: the mail domain should be set<sup>(\*\*)</sup> | **[`arc_parameters` > `arc_signer_domain`](/gpldoc/man/sympa_config.5.html#arc_parametersarc_signer_domain)** |
| [**`arc_selector`**](/gpldoc/man/sympa_config.5.html#arc_selector) | `dkim_selector` | **[`arc_parameters` > `arc_selector`](/gpldoc/man/sympa_config.5.html#arc_parametersarc_selector)** |
| [**`arc_private_key_path`**](/gpldoc/man/sympa_config.5.html#arc_private_key_path) | `dkim_private_key_path` | **[`arc_parameters` > `arc_private_key_path`](/gpldoc/man/sympa_config.5.html#arc_parametersarc_private_key_path)** |
| [**`dmarc_protection_mode`**](/gpldoc/man/sympa_config.5.html#dmarc_protectionmode) | none | [`dmarc_protection` > `mode`](/gpldoc/man/sympa_config.5.html#dmarc_protectionmode) |


### Not yet implemented

| parameter name ([``sympa.conf``](../../layout.md#config) or ``robot.conf`` context) | default | overwritten by (list configuration) |
|---|---|---|
| [dkim_header_list](/gpldoc/man/sympa_config.5.html#dkim_header_list) | as recommended in RFC 6376 | Not yet implemented |


> **Note**
>   * (\*) [`dkim_signer_identity`](/gpldoc/man/sympa_config.5.html#dkim_signer_identity) in
>     `sympa.conf` or `robot.conf` _won't_ be
>     overwritten by `dkim_parameters > signer_identity`,
>     in other words the former is not treated as the default for the latter.
>
>   * (\*\*) On Sympa 6.2.70 or earlier, there was a bug that
>     [`arc_signer_domain`](/gpldoc/man/sympa_config.5.html#arc_signer_domain)
>     and
>     [`dkim_signer_domain`](/gpldoc/man/sympa_config.5.html#dkim_signer_domain)
>     have no default
>     values and they have to be set explicitly.
>     This will be fixed on 6.2.72 so that if these parameters are not
>     set, the recommended value, i.e. the mail domain, is used.

Tests
-----

  * If you enabled DKIM signing, confirm that `DKIM-Signature` field with
    expected signing domain (the same as mail domain) is added
    towards the beginning of the message sent through Sympa.

  * If you enabled ARC, confirm that `ARC-Seal:`, `ARC-Message-Signature:`
    and `ARC-Authentication-Results:` fields with expected authserv-id and
    signing domain are added towards the beginning of message sent
    through Sympa.

> **Note**
>
>   * On Sympa 6.2.72 or later, if ARC feature is enabled,
>     DKIM signature will be added to outgoing messages as necessity,
>     regardless to availability of the DKIM feature.
>
>     Conversely on earlier version of Sympa, ARC sealing is possible only
>     when DKIM signature is allowed by
>     [`dkim_signature_apply_on`](/gpldoc/man/sympa_config.5.html#dkim_signature_apply_on)
>     parameter.


