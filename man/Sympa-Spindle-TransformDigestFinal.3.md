# NAME

Sympa::Spindle::TransformDigestFinal -
Process to transform digest messages - final stage

# DESCRIPTION

[Sympa::Spindle::TransformDigestFinal](./Sympa-Spindle-TransformDigestFinal.3.md) decorates messages bound for list
members with `digest`, `digestplain` or `summary` reception mode.

This class represents the series of following processes:

- Adding RFC 2919 `List-Id:` header field.
- Adding RFC 2369 mailing list header fields.

# SEE ALSO

[Sympa::Message](./Sympa-Message.3.md),
[Sympa::Spindle](./Sympa-Spindle.3.md),
[Sympa::Spindle::ProcessDigest](./Sympa-Spindle-ProcessDigest.3.md).

# HISTORY

[Sympa::Spindle::TransformDigestFinal](./Sympa-Spindle-TransformDigestFinal.3.md) appeared on Sympa 6.2.13.
