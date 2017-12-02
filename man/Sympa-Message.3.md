# NAME

Sympa::Message - Mail message embedding for internal use in Sympa

# SYNOPSYS

    use Sympa::Message;
    my $message = Sympa::Message->new($serialized, context => $list);

# DESCRIPTION

While processing a message in Sympa, we need to link information to the
message, modify headers and such.  This was quite a problem when a message was
signed, as modifying anything in the message body would alter its MD5
footprint. And probably make the message to be rejected by clients verifying
its identity (which is somehow a good thing as it is the reason why people use
MD5 after all). With such messages, the process was complex. We then decided
to embed any message treated in a "Message" object, thus making the process
easier.

## Methods and functions

- new ( $serialized, context => $that, KEY => value, ... )

    _Constructor_.
    Creates a new [Sympa::Message](./Sympa-Message.3.md) object.

    Parameters:

    - $serialized

        Serialized message.

    - context => object

        Context.  [Sympa::List](./Sympa-List.3.md) object, Robot or `'*'`.

    - key => value, ...

        Metadata.

    Returns:

    A new [Sympa::Message](./Sympa-Message.3.md) object, or _undef_, if something went wrong.

- dup ( )

    _Copy constructor_.
    Gets deep copy of instance.

- to\_string ( \[ original => 0|1 \] )

    _Serializer_.
    Returns serialized data of Message object.

    Parameter:

    - original => 0|1

        If set to 1 and content has been decrypted, returns original content.
        Default is 0.

    Returns:

    Serialized representation of Message object.

- add\_header ( $field, $value, \[ $index \] )

    _Instance method_.
    Adds a header field named $field with body $value.
    If $index is given, the field will be inserted at the place it indicates:
    If it is `0`, the field will be prepended.

- delete\_header ( $field, \[ $index \] )

    _Instance method_.
    Deletes all occurrences of the header field named $field.

- replace\_header ( $field, $value, \[ $index \] )

    _Instance method_.
    Replaces header fields named $field with $value.

- head

    _Instance method_.
    Gets header of the message as [MIME::Head](https://metacpan.org/pod/MIME::Head) instance.

    Note that returned value is real reference to internal data structure.
    Even if it was changed, string representation of message may not be updated.
    Alternatively, use ["add\_header"](#add_header)(), ["delete\_header"](#delete_header)() or
    ["replace\_header"](#replace_header)() to modify header.

- check\_spam\_status ( )

    _Instance method_.
    Gets spam status according to spam\_status scenario
    and sets it as {smap\_status} attribute.

- dkim\_sign ( dkim\_d => $d, \[ dkim\_i => $i \],
dkim\_selector => $selector, dkim\_privatekey => $privatekey )

    _Instance method_.
    Adds DKIM signature to the message.

- check\_dkim\_signature ( )

    _Instance method_.
    Checks DKIM signature of the message
    and sets or clears {dkim\_pass} item of the message object.

- remove\_invalid\_dkim\_signature ( )

    _Instance method_.
    Verifys DKIM signatures included in the message,
    and if any of them are invalid, removes them.

- as\_entity ( )

    _Instance method_.
    Gets message content as MIME entity ([MIME::Entity](https://metacpan.org/pod/MIME::Entity) instance).

    Note that returned value is real reference to internal data structure.
    Even if it was changed, string representation of message may not be updated.
    Below is better way to modify message.

        my $entity = $message->as_entity->dup;
        # ... Modify $entity...
        $message->set_entity($entity);

- set\_entity ( $entity )

    _Instance method_.
    Updates message with MIME entity ([MIME::Entity](https://metacpan.org/pod/MIME::Entity) instance).
    String representation will be automatically updated.

- as\_string ( )

    _Instance method_.
    Gets a string representation of message.

    Parameter:

    - original => 0|1

        If set to 1 and content has been decrypted, returns original content.
        Default is 0.

    Note that method like "set\_string()" does not exist:
    You would be better to create new instance rather than replacing entire
    content.

- body\_as\_string ( )

    _Instance method_.
    Gets body of the message as string.

    Note that the result won't be decoded.

- header\_as\_string ( )

    _Instance method_.
    Gets header part of the message as string.

    Note that the result won't be decoded nor unfolded.

- get\_header ( $field, \[ $sep \] )

    _Instance method_.
    Gets value(s) of header field $field, stripping trailing newline.

    **In scalar context** without $sep, returns first occurrence or `undef`.
    If $sep is defined, returns all occurrences joined by it, or `undef`.
    Otherwise **in array context**, returns an array of all occurrences or `()`.

    Note:
    Folding newlines will not be removed.

- get\_decoded\_header ( $tag, \[ $sep \] )

    _Instance method_.
    Returns header value decoded to UTF-8 or undef.
    Trailing newline will be removed.
    If $sep is given, returns all occurrences joined by it.

- dump ( $output )

    _Instance method_.
    Dumps a Message object to a stream.

    Parameters:

    - $output

        the stream to which dump the object

    Returns:

    1. if everything's alright

- add\_topic ( $output )

    Note:
    No longer used.

    _Instance method_.
    Adds topic and puts header X-Sympa-Topic.

    Parameters:

    - $output

        the string containing the topic to add

    Returns:

    1. if everything's alright

- get\_topic ( )

    Note:
    No longer used.

    _Instance method_.
    Gets topic of message.

    Parameters:

    None.

    Returns:

    - the topic

        if it exists

    - empty string

        otherwise

- clean\_html ( )

    _Instance method_.
    Encodes HTML parts of the message by UTF-8 and strips scripts included in
    them.

- smime\_decrypt ( )

    _Instance method_.
    Decrypts message using private key of user.

    Note that this method modifys Message object.

    Parameters:

    None.

    Returns:

    True value if message was decrypted.  Otherwise false value.

    If decrypting succeeded, {smime\_crypted} item is set.

- smime\_encrypt ( $email )

    _Instance method_.
    Encrypts message using certificate of user.

    Note that this method modifys Message object.

    Parameters:

    - $email

        E-mail address of user.

    Returns:

    True value if encryption succeeded, or `undef`.

- smime\_sign ( )

    _Instance method_.
    Adds S/MIME signature to the message.

    Signing key is taken from what stored in list directory.

    Parameters:

    None.

    Returns:

    True value if message was successfully signed.
    Otherwise false value.

- check\_smime\_signature ( )

    _Instance method_.
    Verifys S/MIME signature of the message,
    and if verification succeeded, sets {smime\_signed} item true.

    Parameters:

    None

    Returns:

    1 if signature is successfully verified.
    0 otherwise.
    `undef` if something went wrong.

- personalize ( $list, \[ $rcpt \], \[ $data \] )

    _Instance method_.
    Personalizes a message with custom attributes of a user.

    Parameters:

    - $list

        [List](https://metacpan.org/pod/List) object.

    - $rcpt

        Recipient.

    - $data

        Hashref.  Additional data to be interpolated into personalized message.

    Returns:

    Modified message itself, or `undef` if error occurred.

- test\_personalize ( $list )

    DEPRECATED by Sympa 6.2.13.
    No longer available.

    _Instance method_.
    Tests if personalization can be performed successfully over all subscribers
    of list.

    Parameters:

    Returns:

    `1` if succeed, or `undef`.

- personalize\_text ( $body, $list, \[ $rcpt \], \[ $data \] )

    _Function_.
    Retrieves the customized data of the
    users then parses the text. It returns the
    personalized text.

    Parameters:

    - $body

        Message body with the TT2.

    - $list

        [List](https://metacpan.org/pod/List) object.

    - $rcpt

        The recipient email.

    - $data

        Hashref.  Additional data to be interpolated into personalized message.

    Returns:

    Customized text, or `undef` if error occurred.

- prepare\_message\_according\_to\_mode ( $mode, $list )

    _Instance method_.
    Transforms the message according to reception mode:
    `'mail'`, `'notice'` or `'txt'`.
    Note: 'html' mode was deprecated as of 6.2.23b.2.

    By `'nomail'`, `'digest'`, `'digestplain'` or `'summary'` mode,
    the message is not modified.

    Returns modified message object itself, or `undef` if transformation failed.

- decorate ( )

    OBSOLETED.
    Use prepare\_message\_according\_to\_mode('mail', $list).

    _Instance method_.
    Adds footer/header to a message.

- reformat\_utf8\_message ( )

    _Instance method_.
    Reformats bodies of text parts contained in the message using
    recommended encoding schema and/or charsets defined by MIME::Charset.

    MIME-compliant headers are appended / modified.  And custom X-Mailer:
    header is appended :).

    Parameters:

    - $attachments

        ref(ARRAY) - messages to be attached as subparts.

    Returns:

    string

- get\_plain\_body ( )

    _Instance method_.
    Gets decoded content of text/plain part.

    The text will be converted to UTF-8.
    Flowed text (see RFC 3676) will be conjuncted.

- check\_virus\_infection ( \[ debug => 1 \] )

    _Instance method_.
    Checks the message using anti-virus plugin, if configuration requests it.

    Parameter:

    TBD.

    Returns:

    The name of malware the message contains, if any;
    `"unknown"` for unidentified malware;
    `undef` if checking failed;
    otherwise `0`.

- get\_plaindigest\_body ( )

    _Instance method_.
    Returns a plain text version of message, suitable for use in plain text
    digests.

    - Most attachments are stripped out and replaced with a
    note that they've been stripped. text/plain parts are
    retained.
    - An attempt to convert text/html parts to plain text is made
    if there is no text/plain alternative.
    - All messages are converted from their original character
    set to UTF-8.
    - Parts of type message/rfc822 are recursed
    through in the same way, with brief headers included.
    - Any line consisting only of 30 hyphens has the first
    character changed to space (see RFC 1153). Lines are
    wrapped at 76 columns.

    Parameters:

    None.

    Returns:

    String.

- dmarc\_protect ( )

    _Instance method_.
    Munges the `From:` header field if we are using DMARC Protection mode.

    Parameters:

    None.

    Returns:

    None.
    `From:` field of the message may be modified.

- compute\_topic ( )

    _Instance method_.
    Compute the topic of the message. The topic is got
    from keywords defined in list parameter
    msg\_topic.keywords. The keyword is applied on the
    subject and/or the body of the message according
    to list parameter msg\_topic\_keywords\_apply\_on

    Parameters:

    None.

    Returns:

    String of tag(s), can be separated by ',', can be empty.

- get\_id ( )

    _Instance method_.
    Gets unique identifier of instance.

## Context and Metadata

Context and metadata given to constructor are accessible as hash elements of
object.  These are typically used.

- {context}

    Context of the message, [Sympa::List](./Sympa-List.3.md) object, robot or `'*'`.

- {date}

    The UNIX time messages was initially accepted, or the time message should be
    delivered.

- {domainpart}
- {listname}
- {listtype}
- {localpart}

    Domain, name, type and local part of context.

- {priority}

    Priority of the message.

- {tag}

    Tag of packet used by bulk spool to control logging.
    `'0'` is the first message of multiple packet.
    `'z'` is the last.
    `'s'` is the single message with single packet.

- {time}

    The Unix time in floating point number when the message was stored into the
    spool.  This is used by bulk spool.

## Attributes

These are accessible as hash elements of objects.

- {checksum}

    No longer used.  It is kept for compatibility with Sympa 6.1.x or earlier.
    See also upgrade\_send\_spool(1).

- {envelope\_sender}

    Envelope sender, a.k.a. "Unix From".
    This is not always same as {sender} attribute
    nor the content of `From:` field.

    `'<>'` will be used for "null envelope sender".

- {family}

    Name of family (see [Sympa::Family](./Sympa-Family.3.md)) the message corresponds to.
    This is given by familyqueue(8) program.

- {gecos}

    Display name of actual sender (see {sender} below), if any.

- {md5\_check}

    True value indicates that the message has been authenticated by `md5` level
    (password authentication).
    This is set by web mailer of WWSympa and used by incoming spool.

- {message\_id}

    Original message ID of the message.

- {rcpt}

    Recipients for delivery.
    This is kept for compatibility with earlier releases.

- {sender}

    Actual sender of the message.
    This is determined according to `sender_headers` configuration parameter.
    See also {envelope\_sender} above.

- {shelved}

    Shelved processing.
    Hashref with multiple items.
    Currently these items are available:

    - dkim\_sign => 1

        Adding DKIM signature.

    - dmarc\_protect => 1

        DMARC protection.  See also ["dmarc\_protect"](#dmarc_protect)().

    - merge => 1

        Personalizing.

    - smime\_encrypt => 1

        Adding S/MIME encryption.

    - smime\_sign => 1

        Adding S/MIME signature.

    - tracking => `dsn`|`mdn`|`r`|`w`|`verp`

        Requesting tracking feature including VERP.

    This is used by bulk spool.

- {spam\_status}

    Result of spam check.
    This is set by ["check\_spam\_status"](#check_spam_status)() method.

## Serialization

[Sympa::Message](./Sympa-Message.3.md) object includes number of slots as hash items:
**metadata**, **context**, **attributes** and **message content**.
Metadata including context are given by spool:
See ["Marshaling and unmarshaling metadata" in Sympa::Spool](./Sympa-Spool.3.md#marshaling-and-unmarshaling-metadata).

Logically, objects are stored into physical spool as **serialized form**
and deserialized when they are fetched from spool.
**Attributes** will be serialized and deserialized along with raw message
content.
Attributes are encoded in `X-Sympa-*:` pseudo-header fields and
`Return-Path:` header field.
Below is an example of serialized form.

    X-Sympa-Message-ID: 123456789.12345@domain.name : {message_id} attribute
    X-Sympa-Sender: user01@user.sympa.test          : {sender} attribute
    X-Sympa-Display-Name: Infant                    : {gecos} attribute
    X-Sympa-Shelved: dkim_sign; tracking=mdn        : {shelved} attribute
    X-Sympa-Spam-Status: ham                        : {spam_status} attribute
    Return-Path: sympa-request@domain.name          : {envelope_sender} attribute
    Message-Id: <123456789.12345@domain.name>       :   ---
    From: Infant <user@other.host.dom>              :    |
    To: User <user@some.host.name>                  :    |
    Subject: Howdy world                            :    | Raw message content
    X-Sympa-Topic: sometopic                        :    |
                                                    :    |
    Bonjour, le monde.                              :    |
                                                    :   ---

On msg, automatic and bounce spools,
`Return-Path:` header fields are given by MDA
and `X-Sympa-*:` header fields are given by queue programs.
On other spools, they are given by components of Sympa.

Pseudo-header fields _should_ appear at beginning of serialized content.
Fields appear at other places (e.g. `X-Sympa-Topic:` field above) are not
attributes but are the part of raw message content.

Pseudo-header fields _should not_ be included in actually sent messages.

# CAVEAT

## Adding `Return-Path:` field

We trust in `Return-Path:` header field only at the top of message
to prevent forgery.  To ensure it will be added to messages by MDA,

- Sendmail

    Add `P` in the `F=` flags of local mailer line (such as `Mlocal`).

- Postfix
    - local(8)

        Prepending `Return-Path:` is available by default.

    - pipe(8)

        Add `R` to the `flags=` attributes in master.cf.
- Exim

    Set `return_path_add` to be true with pipe\_transport.

- qmail

    Use preline(1).

- sympa-milter

    As of version 0.7, prepending `Return-Path:` is available.

# BUGS

[get\_plaindigest\_body](https://metacpan.org/pod/get_plaindigest_body)()
seems to ignore any text after a UUencoded attachment.

# HISTORY

[Message](https://metacpan.org/pod/Message) module appeared on Sympa 3.3.6.
It was initially written by:

- Serge Aumont &lt;sa AT cru.fr>
- Olivier Salaün &lt;os AT cru.fr>

[get\_plaindigest\_body](https://metacpan.org/pod/get_plaindigest_body), ex. ["plain\_body\_as\_string" in PlainDigest](https://metacpan.org/pod/PlainDigest#plain_body_as_string),
was initially written by Chris Hastie.  It appeared on Sympa 4.2b.1.

    (c) Chris Hastie 2004 - 2008.

Renamed and merged [Sympa::Message](./Sympa-Message.3.md) appeard on Sympa 6.2.
