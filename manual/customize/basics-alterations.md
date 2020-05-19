---
title: 'Message alteration'
prev: basics-acceptance.md
up: basics-workflow.md
next: basics-delivery.md
---

Message alteration
==================

Sympa's [`sympa_msg.pl`](/gpldoc/man/sympa_msg.8.html) daemon fetches incoming
messages on the incoming spool, [``$SPOOLDIR``](../layout.md#spooldir)`/msg`
directory.  Then it creates an memory image of the message
for later processing.  At the end, the processed image is stored into
outgoing spool, [``$SPOOLDIR``](../layout.md#spooldir)`/bulk` directory,
and [`bulk.pl`](/gpldoc/man/bulk.8.html) daemon fetches and dumps it to the mail
transfer agent (MTA).

Does Sympa alter messages?
--------------------------

The intermediate image described above might be altered.  However, if a
message is S/MIME signed, it should not be altered, because any changes in any
part of the message body would break integrity of the S/MIME signature.

Sympa might perform the following changes to the messages to be distributed.
Some of these alterations can be configured.

### Decrypting

  - If a message is encrypted and S/MIME certificate of originator is valid,
    decryption is tried.  Later this message may be encrypted again
    (see below).

### Altering message header

  - `X-Sympa-Topic` header field is added as necessity.

    See "[Message topics](basics-delivery.md#message-topics)".

  - If anonymization mode is enabled, several header fields are removed or
    consealed.

    See [`anonymous_sender`](/gpldoc/man/list_config.5.html#anonymous_sender) and
    [`anonymous_header_fields`](/gpldoc/man/sympa.conf.5.html#anonymous_header_fields)
    parameters.

  - The subject of the message might be changed to add a custom subject tag.

    See [`custom_subject`](/gpldoc/man/list_config.5.html#custom_subject).

  - Additional header fields are removed according to customizations.

    See [`remove_headers`](/gpldoc/man/list_config.5.html#remove_headers) and
    [`remove_outgoing_headers`](/gpldoc/man/list_config.5.html#remove_outgoing_headers).

    ----
    Note:

      * `remove_headers` does not remove header fields described after this,
        but `remove_outgoing_headers` can do.  Use the latter one only when
        you know what you are doing, or messages might not be distributed
        correctly.

    ----

  - `Reply-To` header field is altered according to configuration.

     See [`reply_to_header`](/gpldoc/man/list_config.5.html#reply_to_header).

  - `X-Sequence` header field is added.  Its value is the sequence number of
     posts.

  - Some header fields are added/removed to prevent mail loop.

    See "[Loop prevention](basics-acceptance#loop-prevention)".

  - `Sender` header field is added/altered.  It should have appropriate value
    to satisfy some sender domain validation systems such as DKIM, Sender ID.

  - Header fields configured by
    [`custom_header`](/gpldoc/man/list_config.5.html#custom_header) are added.

  - Some mailing list header fields for
    [RFC 2369](https://tools.ietf.org/html/rfc2369) compliance (see
    [`rfc2369_header_fields`](/gpldoc/man/list_config.5.html#rfc2369_header_fields))
    are added.
    Also, `List-Id` ([RFC 2919](https://tools.ietf.org/html/rfc2919)) and
    `Archived-At` ([RFC 5064](https://tools.ietf.org/html/rfc5064)) fields are
    added.

### Altering message body

  - Message body is altered according to message reception mode chosen by each
    recipient.

    See "[Message reception modes](#message-reception-modes)" for details.

  - If message personalization is enabled, message body is altered further.

    See "~~[Message personalization](../customize/web-mailer.md#message-personalization)~~".

### Encrypting and signing

  - If DMARC protection is enabled, `From` header field is consealed.

    See "[DMARC protection](../customize/dmarc-protection.md)".

  - If original message has been decrypted (i.e. originally encrypted:
    See above), re-encryption using certificate of recipient is tried.
    When it fails, a message to inform failure instead of original message is
    sent to recipient.

    See "[S/MIME](../customize/smime.md)" for further details.

  - If DKIM support is enabled, DKIM signature invalidated by alterations
    so far is removed, then message is signed using Sympa's private key.

    See "[DKIM features for Sympa](../customize/dkim-arc.md)".

Eventually, the message is delivered to recipient by the MTA.

Message reception modes
-----------------------

List members can choose the reception mode of their own either through the web
interface or via the `SET `*list*` `*mode* mail command.

The available reception modes can be restricted by listmasters and/or list
owners with the
[`available_user_options`](/gpldoc/man/list_config.5.html#available_user_options)
list/global parameter.  list owners can define default reception mode for
users added to the list with
[`default_user_options`](/gpldoc/man/list_config.5.html#default_user_options)
list parameter.

Following changes are made by each mode:

### Regular delivery

  - `mail`:
    Keeps original content of the message.
  - `notice`:
    Keeps only the subject of the message.  Message body is entirely removed.
  - `txt`:
    Keeps only plain text part of `multipart/alternative` message.
    Message content-type is changed from multipart/alternative to
    text/plain.
  - `urlize`:
    Replaces attachments with the links to the file in message archive.
    This "urlizization" depends on the size of each message part: See also
    [`urlize_min_size`](/gpldoc/man/list_config.5.html#urlize_min_size) parameter.

  - `not_me`:
    Same as `mail`, however if the recipient is originator of the message,
    no delivery.
  - `nomail`:
    No delivery.

Additionally with `mail`, `not_me`, `txt` or `urlize` mode,
"header" and/or "footer" may be added. They are either added as separate MIME
parts, or within the message body if it is of text type.
See
"[Message header and footer](basics-list-config.md#message-header-and-footer)").

*However*, `mail` and `not_me` modes do not alter S/MIME signed message (i.e.
its MIME type is `multipart/signed`) so that integrity of signature will not
be broken.

----
Note:

  * `html` reception mode was deprecated on Sympa 6.2.24. Like `txt` mode,
    this mode intended to keep only *HTML part* of multipart messages, and
    therefore not practically useful.

----

### Digest delivery

If the recipient chooses one of following digest delivery modes, multiple
messages are compiled in one message and delivered periodically.

  - `digest`:
    Digest delivery (MIME `multipart/digest` format).
  - `digestplain`:
    Digest delivery (plain text format according to RFC 1153).
  - `summary`:
    Digest delivery, sort of: Sends the list of links to message archive.

With `digest` mode, the body of each message compiled in is not altered.
the other modes does not keep signature.

*However*, encrypted messages are never included in compiled message.

