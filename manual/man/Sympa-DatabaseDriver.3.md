# NAME

Sympa::DatabaseDriver - Base class of database drivers for Sympa

# SYNOPSIS

    package Sympa::DatabaseDriver::FOO;
    use base qw(Sympa::DatabaseDriver);

# DESCRIPTION

[Sympa::DatabaseDriver](./Sympa-DatabaseDriver.3.md) is the base class of driver classes for
Sympa Database Manager (SDM).

## Instance methods subclasses should implement

- required\_modules ( )

    _Overridable_.
    Returns an arrayref including package name(s) this driver requires.
    By default, no packages are required.

- required\_parameters ( )

    _Overridable_.
    Returns an arrayref including names of required (not optional) parameters.
    By default, returns `['db_host', 'db_name', 'db_user']`.

- optional\_modules ( )

    _Overridable_.
    Returns an arrayref including all name(s) of optional packages.
    By default, there are no optional packages.

    This method was introduced by Sympa 6.2.4.

- optional\_parameters ( )

    _Overridable_.
    Returns an arrayref including all names of optional parameters.
    By default, returns `'db_passwd'`, `'db_port'`, `'db_options'` and so on.

- build\_connect\_string ( )

    _Mandatory for SQL driver_.
    Builds the string to be used by the DBI to connect to the database.

    Parameter:

    None.

    Returns:

    String representing data source name (DSN).

- connect ( )

    _Overridable_.
    Connects to database calling ["\_connect"](#_connect)() and sets database handle.

    Parameter:

    None.

    Returns:

    True value or, if connection failed, false value.

- \_connect ( )

    _Overridable_.
    Connects to database and returns native database handle.

    The default implementation is for [DBI](https://metacpan.org/pod/DBI) database handle.

- get\_substring\_clause ( { source\_field => $source\_field,
separator => $separator, substring\_length => $substring\_length } )

    This method was deprecated by Sympa 6.2.4.

- get\_limit\_clause ( )

    This method was deprecated.

- get\_formatted\_date ( { mode => $mode, target => $target } )

    _Mandatory for SQL driver_.
    Returns a character string corresponding to the expression to use in a query
    involving a date.

    Parameters:

    - $mode

        authorized values:

        - `'write'`

            The sub returns the expression to use in 'INSERT' or 'UPDATE' queries.

        - `'read'`

            The sub returns the expression to use in 'SELECT' queries.

    - $target

        The name of the field or the value to be used in the query.

    Returns:

    The formatted date or `undef` if the date format mode is unknown.

- is\_autoinc ( { table => $table, field => $field } )

    _Required to probe database structure_.
    Checks whether a field is an auto-increment field or not.

    Parameters:

    - $field

        The name of the field to test

    - $table

        The name of the table to add

    Returns:

    True if the field is an auto-increment field, false otherwise

- set\_autoinc ( { table => $table, field => $field } )

    _Required to update database structure_.
    Defines the field as an auto-increment field.

    Parameters:

    - $field

        The name of the field to set.

    - $table

        The name of the table to add.

    Returns:

    `1` if the auto-increment could be set, `undef` otherwise.

- get\_tables ( )

    _Required to probe database structure_.
    Returns the list of the tables in the database.

    Parameters:

    None.

    Returns:

    A ref to an array containing the list of the table names in the
    database, `undef` if something went wrong.

- add\_table ( { table => $table } )

    _Required to update database structure_.
    Adds a table to the database.

    Parameter:

    - $table

        The name of the table to add

    Returns:

    A character string report of the operation done or `undef` if something
    went wrong.

- get\_fields ( { table => $table } )

    _Required to probe database structure_.
    Returns a ref to an hash containing the description of the fields in a table
    from the database.

    Parameters:

    - $table

        The name of the table whose fields are requested.

    Returns:

    A hash in which the keys are the field names and the values are the field type.

    Returns `undef` if something went wrong.

- update\_field ( { table => $table, field => $field, type => $type, ... } )

    _Required to update database structure_.
    Changes the type of a field in a table from the database.

    Parameters:

    - $field

        The name of the field to update.

    - $table

        The name of the table whose fields will be updated.

    - $type

        The type of the field to add.

    - $notnull

        Specifies that the field must not be null

    Returns:

    A character string report of the operation done or `undef` if something
    went wrong.

- add\_field ( { table => $table, field => $field, type => $type, ... } )

    _Required to update database structure_.
    Adds a field in a table from the database.

    Parameters:

    - $field

        The name of the field to add.

    - $table

        The name of the table where the field will be added.

    - $type

        The type of the field to add.

    - $notnull

        Specifies that the field must not be null.

    - $autoinc

        Specifies that the field must be auto-incremental.

    - $primary

        Specifies that the field is a key.

    Returns:

    A character string report of the operation done or `undef` if something
    went wrong.

- delete\_field ( { table => $table, field => $field } )

    _Required to update database structure_.
    Deletes a field from a table in the database.

    Parameters:

    - $field

        The name of the field to delete

    - $table

        The name of the table where the field will be deleted.

    Returns:

    A character string report of the operation done or `undef` if something
    went wrong.

- get\_primary\_key ( { table => $table } )

    _Required to probe database structure_.
    Returns the list fields being part of a table's primary key.

    - $table

        The name of the table for which the primary keys are requested.

    Returns:

    A ref to a hash in which each key is the name of a primary key or `undef`
    if something went wrong.

- unset\_primary\_key ( { table => $table } )

    _Required to update database structure_.
    Drops the primary key of a table.

    Parameter:

    - $table

        The name of the table for which the primary keys must be
        dropped.

    Returns:

    A character string report of the operation done or `undef` if something
    went wrong.

- set\_primary\_key ( { table => $table, fields => $fields } )

    _Required to update database structure_.
    Sets the primary key of a table.

    Parameters:

    - $table

        The name of the table for which the primary keys must be
        defined.

    - $fields

        A ref to an array containing the names of the fields used
        in the key.

    Returns:

    A character string report of the operation done or `undef` if something
    went wrong.

- get\_indexes ( { table => $table } )

    _Required to probe database structure_.
    Returns a ref to a hash in which each key is the name of an index.

    Parameter:

    - $table

        The name of the table for which the indexes are requested.

    Returns:

    A ref to a hash in which each key is the name of an index.  These key
    point to a second level hash in which each key is the name of the field
    indexed.  Returns `undef` if something went wrong.

- unset\_index ( { table => $table, index => $index } )

    _Required to update database structure_.
    Drops an index of a table.

    Parameters:

    - $table

        The name of the table for which the index must be dropped.

    - $index

        The name of the index to be dropped.

    Returns:

    A character string report of the operation done or `undef` if something
    went wrong.

- set\_index ( { table => $table, index\_name => $index\_name,
fields => $fields } )

    _Required to update database structure_.
    Sets an index in a table.

    Parameters:

    - $table

        The name of the table for which the index must be defined.

    - $fields

        A ref to an array containing the names of the fields used
        in the index.

    - $index\_name

        The name of the index to be defined.

    Returns:

    A character string report of the operation done or `undef` if something
    went wrong.

- translate\_type ( $generic\_type )

    _Required to probe and update database structure_.
    Get native field type corresponds to generic type.
    The generic type is based on MySQL:
    See ["full\_db\_struct" in Sympa::DatabaseDescription](./Sympa-DatabaseDescription.3.md#full_db_struct).

Subclasses of [Sympa::DatabaseDriver](./Sympa-DatabaseDriver.3.md) class also can override methods
provided by [Sympa::Database](./Sympa-Database.3.md) class:

- do\_operation ( $operation, $parameters, ...)

    _Overridable_, _only for LDAP driver_.

- do\_query ( $query, $parameters, ... )

    _Overridable_, _only for SQL driver_.

- do\_prepared\_query ( $query, $parameters, ... )

    _Overridable_, _only for SQL driver_.

- AS\_DOUBLE ( $value )

    _Overridable_.
    Helper functions to return the DOUBLE binding type and value used by
    ["do\_prepared\_query"](#do_prepared_query)().
    Overridden by inherited classes.

    Parameter:

    - $value

    Parameter value

    Returns:

    One of:

    - An array `( { sql_type => SQL_type }, value )`.
    - Single value (i.e. an array with single item), if special
    treatment won't be needed.
    - Empty array `()` if arguments were not given.

- AS\_BLOB ( $value )

    _Overridable_.
    Helper functions to return the BLOB (binary large object) binding type and
    value used by ["do\_prepared\_query"](#do_prepared_query)().
    Overridden by inherited classes.

    See ["AS\_DOUBLE"](#as_double) for more details.

## Utility method

- \_\_dbh ( )

    _Instance method_, _protected_.
    Returns native database handle which [\_connect](https://metacpan.org/pod/_connect)() returned.
    This may be used at inside of each driver class.

# SEE ALSO

[Sympa::Database](./Sympa-Database.3.md), [Sympa::DatabaseManager](./Sympa-DatabaseManager.3.md).

# HISTORY

Sympa Database Manager (SDM) appeared on Sympa 6.2.
