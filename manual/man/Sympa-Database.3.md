---
title: 'Sympa::Database(3)'
---

# NAME

Sympa::Database - Handling databases

# SYNOPSIS

    use Sympa::Database;

    $database = Sympa::Database->new('SQLite', db_name => '...');
        or die 'Cannot connect to database';
    $sth = $database->do_prepared_query('SELECT FROM ...', ...)
        or die 'Cannot execute query';
    $database->disconnect;

# DESCRIPTION

TBD.

## Methods

- new ( $db\_type, \[ option => value, ... \] )

    _Constructor_.
    Creates new database instance.

- do\_operation ( $operation, options... )

    _Instance method_, _only for LDAP_.
    Performs LDAP search operation.
    About options see ["search" in Net::LDAP](https://metacpan.org/pod/Net::LDAP#search).

    Returns:

    Operation handle ([LDAP::Search](https://metacpan.org/pod/LDAP::Search) object or such), or `undef`.

- do\_prepared\_query ( $statement, parameters... )

    _Instance method_, _only for SQL_.
    Prepares and executes SQL query.
    $statement is an SQL statement that may contain placeholders `?`.

    Returns:

    Statement handle ([DBI::st](https://metacpan.org/pod/DBI::st) object or such), or `undef`.

- do\_query ( $statement, parameters... )

    _Instance method_, _only for SQL_.
    Executes SQL query.
    $statement and parameters will be fed to sprintf().

    Returns:

    Statement handle ([DBI::st](https://metacpan.org/pod/DBI::st) object or such), or `undef`.

# SEE ALSO

[Sympa::DatabaseDriver](./Sympa-DatabaseDriver.3.md), [Sympa::Datasource](./Sympa-Datasource.3.md).

# HISTORY

Sympa Database Manager (SDM) appeared on Sympa 6.2.
