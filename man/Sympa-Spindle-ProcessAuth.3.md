# NAME

Sympa::Spindle::ProcessAuth - Workflow of request confirmation

# SYNOPSIS

    use Sympa::Spindle::ProcessAuth;

    my $spindle = Sympa::Spindle::ProcessAuth->new(
        confirmed_by => $email, context => $robot, keyauth => $key,
        scenario_context => {sender => $sender});
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessAuth](./Sympa-Spindle-ProcessAuth.3.md) defines workflow for confirmation of held
requests.

When spin() method is invoked, it reads a request in held request spool,
authorizes it and dispatch it if possible.
Either authorization and dispatching failed or not, spin() will terminate
processing.
Failed request will be kept in spool and wait for confirmation again.

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

- new ( confirmed\_by => $email,
context => $context, keyauth => $key,
\[ quiet => 1 \], scenario\_context => {context...} )
- spin ( )

    new() must take following options:

    - confirmed\_by => $email

        E-mail address of the user who confirmed the request.
        It is given by AUTH command and
        used by [Sympa::Spindle::AuthorizeRequest](./Sympa-Spindle-AuthorizeRequest.3.md) to execute scenario.

    - context => $context
    - keyauth => $key

        Context (List or Robot) and authorization key to specify the request in
        spool.

    - quiet => 1

        If this option is set, automatic replies reporting result of processing
        to the user (see ["confirmed\_by"](#confirmed_by)) will not be sent.

    - scenario\_context => {context...}

        Authorization context given to scenario.

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa-Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Spool::Auth](./Sympa-Spool-Auth.3.md) class.

- {finish}

    `'success'` is set if processing succeeded.
    `'failure'` is set if processing failed.

# SEE ALSO

[Sympa::Request](./Sympa-Request.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spindle::AuthorizeRequest](./Sympa-Spindle-AuthorizeRequest.3.md),
[Sympa::Spool::Auth](./Sympa-Spool-Auth.3.md).

# HISTORY

[Sympa::Spindle::ProcessAuth](./Sympa-Spindle-ProcessAuth.3.md) appeared on Sympa 6.2.15.
