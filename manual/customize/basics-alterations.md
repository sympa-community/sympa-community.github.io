Message alteration
==================

Sympa's [`sympa_msg.pl`](../man/sympa_msg.8.md) daemon fetches incoming
messages on the incoming spool, [``$SPOOLDIR``](../layout.md#spooldir)`/msg`
directory.  Then it creates an memory image of the message
for later processing.  At the end, the processed image is stored into
outgoing spool, [``$SPOOLDIR``](../layout.md#spooldir)`/bulk` directory,
and [`bulk.pl`](../man/bulk.8.md) daemon fetches and dumps it to the mail
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

    See [`anonymous_sender`](../man/list_config.5.md#anonymous_sender) and
    [`anonymous_header_fields`](../man/sympa.conf.5.md#anonymous_header_fields)
    parameters.

  - The subject of the message might be changed to add a custom subject tag.

    See [`custom_subject`](../man/list_config.5.md#custom_subject).

  - Additional header fields are removed according to customizations.

    See [`remove_headers`](../man/list_config.5.md#remove_headers) and
    [`remove_outgoing_headers`](../man/list_config.5.md#remove_outgoing_headers).

    ----
    Note:

      * `remove_headers` does not remove header fields described after this,
        but `remove_outgoing_headers` can do.  Use the latter one only when
        you know what you are doing, or messages might not be distributed
        correctly.

    ----

  - `Reply-To` header field is altered according to configuration.

     See [`reply_to_header`](../list_config.5.md#reply_to_header).

  - `X-Sequence` header field is added.  Its value is the sequence number of
     posts.

  - Some header fields are added/removed to prevent mail loop.

    See "~~[Loop prevention](../customize/loop-prevention.md)~~".

  - `Sender` header field is added/altered.  It should have appropriate value
    to satisfy some sender domain validation systems such as DKIM, Sender ID.

  - Header fields configured by
    [`custom_header`](../list_config.5.md#custom_header) are added.

  - Some mailing list header fields for
    [RFC 2369](https://tools.ietf.org/html/rfc2369) compliance (see
    [`rfc2369_header_fields`](../man/list_config.5.md#rfc2369_header_fields))
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

    See "[DMRAC protection](../customize/dmarc-protection.md)".

  - If original message has been decrypted (i.e. originally encrypted:
    See above), re-encryption using certificate of recipient is tried.
    When it fails, a message to inform failure instead of original message is
    sent to recipient.

    See "~~[S/MIME](../customize/smime.md)~~" for further details.

  - If DKIM support is enabled, DKIM signature invalidated by alterations
    so far is removed, then message is signed using Sympa's private key.

    See "[DKIM features for Sympa](../customize/dkim.md)".

Eventually, the message is delivered to recipient by the MTA.

Message reception modes
-----------------------

  - Headers and footers may be added. They are either added as separate MIME
    parts or within the message body, if it is of text type (see
    "~~[Message header and footer](/manual/list-definition#message_header_and_footer)~~").

  - Message content-type may be changed from multipart/alternative to
    text/plain if the user has selected the `txt` reception mode.

Attachments
-----------

Sympa distribution process copes well with MIME messages, including those
including attachments. However there are situations special processing for
messages with attachments:

  - [`max_size`](../man/list_config.5.md#max_size) : this parameter controls
    the maximum allowed size for a distributed message,

  - urlize : list members can select this reception mode to have attachments
    removed from the message and stored on the list server. Urlizization
    depends on the message size and the
    [`urlize_min_size`](../man/list_config.5.md#urlize_min_size) parameter.

Loop prevention
---------------

