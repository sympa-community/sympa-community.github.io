# NAME

Sympa::Message::Template - Mail message generated from template

# SYNOPSYS

    use Sympa::Message::Template;
    my $message = Sympa::Message::Template->new(
        context => $list, template => "name", rcpt => [$email], data => {});

# DESCRIPTION

## Methods

- new ( context => $that, template => $filename,
rcpt => $rcpt, \[ data => $data \], \[ options... \] )

    _Constructor_.
    Creates [Sympa::Message](./Sympa::Message.3.md) object from template.

    Parameters:

    - context => $that

        Content: Sympa::List, robot or '\*'.

    - template => $filename

        Template filename (without extension).

    - rcpt => $rcpt

        Scalar or arrayref: SMTP "RCPT TO:" field.

        If it is a scalar, trys to retrieve information of the user
        (See also [Sympa::User](./Sympa::User.3.md).

    - data => $data

        Hashref used to parse template, with keys:

        - return\_path

            SMTP "MAIL FROM:" field if sent by SMTP (see [Sympa::Mailer](./Sympa::Mailer.3.md)),
            "Return-Path:" field if sent by spool.

            Note: This parameter was OBSOLETED.  Currently, {envelope\_sender} attribute of
            object is taken from the context.

        - to

            "To:" header field

        - lang

            Language tag used for parsing template.
            See also [Sympa::Language](./Sympa::Language.3.md).

        - from

            "From:" field if not a full msg

            Note:
            This parameter was OBSOLETED.
            The "From:" field will be filled in by "sympa" address if it is not found.

        - subject

            "Subject:" field if not a full msg

        - replyto

            "Reply-To:" field if not a full msg

        - body

            Body message if $filename is `''`.

            Note: This feature has been deprecated.

        - headers

            Additional headers, hashref with keys are field names.

    Below are optional parameters.

    - date => $time

        Delivery time of message.
        By default current time will be used.

    - envelope\_sender => $email

        Forces setting envelope sender.
        `'<>'` may be used for null envelope sender.

    - priority => $priority

        Forces setting priority if specified.

    - tracking => $feature

        Forces tracking if specified.

    Returns:

    New [Sympa::Message](./Sympa::Message.3.md) instance, or `undef` if something went wrong.

# SEE ALSO

[Sympa::Message](./Sympa::Message.3.md), [Sympa::Template](./Sympa::Template.3.md).

# HISTORY

["new\_from\_template" in Sympa::Message](./Sympa::Message.3.md#new_from_template) appeared on Sympa 6.2.

It was renamed to ["new" in Sympa::Message::Template](./Sympa::Message::Template.3.md#new) on Sympa 6.2.13.
