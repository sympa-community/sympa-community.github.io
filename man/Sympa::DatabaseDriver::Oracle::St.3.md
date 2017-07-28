# NAME

Sympa::DatabaseDriver::Oracle::St - Correcting behavior of DBD::Oracle

# DESCRIPTION

If `NLS_LANG` environment variable is properly set with charset
`AL32UTF8` (or `UTF8`), [DBD::Oracle](https://metacpan.org/pod/DBD::Oracle) handles character values as
Unicode, i.e. "utf8 flags" are set.  This behavior is not desirable for Sympa.

Sympa::DatabaseDriver::Oracle::St overrides functions of DBI statement handle
object to reset utf8 flags.

# HISTORY

[Sympa::DatabaseDriver::Oracle::St](./Sympa::DatabaseDriver::Oracle::St.3.md) appears on Sympa 6.2.
