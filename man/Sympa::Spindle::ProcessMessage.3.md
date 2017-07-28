# NAME

Sympa::Spindle::ProcessMessage - Workflow of command processing

# SYNOPSIS

    use Sympa::Spindle::ProcessMessage;

    my $spindle = Sympa::Spindle::ProcessMessage->new(
        message => $message, scenario_context => {sender => $sender});
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessMessage](./Sympa::Spindle::ProcessMessage.3.md) defines workflow to process commands in
message.

When spin() method is invoked, it parses message and fetch requests and
processes them.

TBD.

-

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa::Spindle.3.md#public-methods).

- new ( scenario\_context => {context...}, \[ message => $message \] )
- spin ( )

    new() may take following options:

    - scenario\_context => {context...}

        Authorization context given to scenario.

    - message => $message

        See [Sympa::Request::Message](./Sympa::Request::Message.3.md).

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa::Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Request::Message](./Sympa::Request::Message.3.md) class.

# SEE ALSO

[Sympa::Request](./Sympa::Request.3.md),
[Sympa::Request::Message](./Sympa::Request::Message.3.md),
[Sympa::Spindle](./Sympa::Spindle.3.md).

# HISTORY

[Sympa::Spindle::ProcessMessage](./Sympa::Spindle::ProcessMessage.3.md) appeared on Sympa 6.2.13.
