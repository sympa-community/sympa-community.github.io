# NAME

Sympa::Spindle::AuthorizeRequest -
Workflow to authorize requests in command messages

# DESCRIPTION

[Sympa::Spindle::AuthorizeRequest](./Sympa-Spindle-AuthorizeRequest.3.md) authorizes requests and stores them
into request spool or dispatch them.

TBD

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa-Spindle.3.md#public-methods).

- new ( key => value, ... )

    In most cases, [Sympa::Spindle::ProcessMessage](./Sympa-Spindle-ProcessMessage.3.md)
    splices meessages to this class.  This method is not used in ordinal case.

- spin ( )

    Not implemented.

# SEE ALSO

[Sympa::Request](./Sympa-Request.3.md), [Sympa::Scenario](./Sympa-Scenario.3.md), [Sympa::Spindle::DispatchRequest](./Sympa-Spindle-DispatchRequest.3.md),
[Sympa::Spindle::ProcessMessage](./Sympa-Spindle-ProcessMessage.3.md), [Sympa::Spindle::ProcessRequest](./Sympa-Spindle-ProcessRequest.3.md),
[Sympa::Spindle::ToAuth](./Sympa-Spindle-ToAuth.3.md), [Sympa::Spindle::ToAuthOwner](./Sympa-Spindle-ToAuthOwner.3.md).

# HISTORY

[Sympa::Spindle::AuthorizeRequest](./Sympa-Spindle-AuthorizeRequest.3.md) appeared on Sympa 6.2.13.
