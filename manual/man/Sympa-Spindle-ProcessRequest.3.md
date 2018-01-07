---
title: 'Sympa::Spindle::ProcessRequest(3)'
---

# NAME

Sympa::Spindle::ProcessRequest - Workflow of request processing

# SYNOPSIS

    use Sympa::Spindle::ProcessRequest;

    my $spindle = Sympa::Spindle::ProcessRequest->new(
        context => $robot, [options...],
        scenario_context => {sender => $sender});
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessRequest](./Sympa-Spindle-ProcessRequest.3.md) defines workflow to process requests.

When spin() method is invoked, it generates requests and processes them.

TBD.

-

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

- new ( options, ..., scenario\_context => {context...} )
- spin ( )

    new() may take following options:

    - options, ...

        Context (List or Robot) and other options to generate the requests.
        See [Sympa::Request](./Sympa-Request.3.md) for more details.

        If one of their value is arrayref, repeatedly generates instances over each
        array item.

    - scenario\_context => {context...}

        Authorization context given to scenario.

## Properties

See also ["Properties" in Sympa::Spindle](./Sympa-Spindle.3.md#properties).

- {distaff}

    Instance of [Sympa::Request::Collection](./Sympa-Request-Collection.3.md) class.

# SEE ALSO

[Sympa::Request](./Sympa-Request.3.md),
[Sympa::Request::Collection](./Sympa-Request-Collection.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spindle::AuthorizeRequest](./Sympa-Spindle-AuthorizeRequest.3.md),

# HISTORY

[Sympa::Spindle::ProcessRequest](./Sympa-Spindle-ProcessRequest.3.md) appeared on Sympa 6.2.15.
