# NAME

Sympa::Spindle::ProcessTemplate - Workflow of template sending

# SYNOPSIS

    use Sympa::Spindle::ProcessTemplate;

    my $spindle = Sympa::Spindle::ProcessTemplate->new( options... );
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessTemplate](./Sympa-Spindle-ProcessTemplate.3.md) defines workflow to send messages
generated from template.

When spin() method is invoked, it takes an message generated from template,
sends the message using another outgoing spindle and returns.

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

- new ( _template options_, \[ splicing\_to => \[spindles\] \],
\[ add\_list\_statistics => 1 \] )
- spin ( )

    new() may take following options.

    - _template options_

        See ["new" in Sympa::Message::Template](./Sympa-Message-Template.3.md#new).

    - splicing\_to => \[spindles\]

        A reference to array containing [Sympa::Spindle](./Sympa-Spindle.3.md) subclass(es) by which
        the message will be sent.
        By default `['Sympa::Spindle::ToOutgoing']` is used.

    - add\_list\_statistics => 1

        TBD.

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa-Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Message::Template](./Sympa-Message-Template.3.md) class.

- {finish}

    `'success'` is set if processing succeeded.
    `'failure'` is set if processing failed.

# SEE ALSO

[Sympa::Message::Template](./Sympa-Message-Template.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md),
[Sympa::Spindle::ToAlarm](./Sympa-Spindle-ToAlarm.3.md), [Sympa::Spindle::ToMailer](./Sympa-Spindle-ToMailer.3.md),
[Sympa::Spindle::ToOutgoing](./Sympa-Spindle-ToOutgoing.3.md).

# HISTORY

[Sympa::Spindle::ProcessTemplate](./Sympa-Spindle-ProcessTemplate.3.md) appeared on Sympa 6.2.13.
