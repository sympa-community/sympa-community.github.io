# NAME

Sympa::Request::Handler::auth - auth request handler

# DESCRIPTION

Fetchs the request matching with {authkey} and optional {request} attributes
from held request spool,
and if succeeded, processes it with `md5` authentication level.

# CAVEAT

Auth request handler itself never check privileges:
It trust in senders if valid authorization key is specified.
Access to this handler should be restricted sufficiently by applications.

# SEE ALSO

[Sympa::Request::Handler](./Sympa-Request-Handler.3.md),
[Sympa::Request::Handler::decl](./Sympa-Request-Handler-decl.3.md),
[Sympa::Spindle::ProcessAuth](./Sympa-Spindle-ProcessAuth.3.md).

# HISTORY

[Sympa::Request::Handler::auth](./Sympa-Request-Handler-auth.3.md) appeared on Sympa 6.2.15.
