# NAME

Sympa::Spindle::DispatchRequest -
Workflow to dispatch requests

# DESCRIPTION

[Sympa::Spindle::DispatchRequest](./Sympa::Spindle::DispatchRequest.3.md) dispatches requests, in most cases
included in command messages.

Requests are dispatched to routines to perform abstruct processing.

## Public methods

See also ["Public methods" in Sympa::Spindle](./Sympa::Spindle.3.md#public-methods).

- new ( key => value, ... )

    In most cases, [Sympa::Spindle::ProcessMessage](./Sympa::Spindle::ProcessMessage.3.md)
    splices requests to this class.  This method is not used in ordinal case.

- spin ( )

    Not implemented.

# SEE ALSO

[Sympa::Spindle](./Sympa::Spindle.3.md), [Sympa::Spindle::ProcessMessage](./Sympa::Spindle::ProcessMessage.3.md),
[Sympa::Spindle::ProcessRequest](./Sympa::Spindle::ProcessRequest.3.md).

# HISTORY

[Sympa::Spindle::DispatchRequest](./Sympa::Spindle::DispatchRequest.3.md) appeared on Sympa 6.2.13.
