---
title: 'Sympa::DatabaseDescription(3)'
---

# NAME

Sympa::DatabaseDescription - Definition of core database structure

# DESCRIPTION

This module keeps structure of database used by Sympa software.

## Functions

- full\_db\_struct ()

    _Function_.
    Returns a heshref containing definitions of all tables.
    Each item has the name of table as key and definition as value.

    Each definition is hashref containing following keys:

    - fields

        See below.

    - doc

        Description of the table.

    - order

        TBD.

    `fields` item is hashref which may contain following items.

    - struct

        Column data types.  Definitions are based on MySQL.
        Following types are recognized:

        - varchar(_length_)

            Text with length up to _length_.  _length_ must be lower than 2^16 - 2.

        - int(1)

            Boolean, 1 or 0.

        - int(11)

            Unix time.

        - int(_cols_)

            Integer with columns up to _cols_, with its value from -2^31 to 2^31 - 1.

        - tinyint

            Integer, -2^7 to 2^7 - 1.

        - smallint

            Integer, -2^15 to 2^15 - 1.

        - bigint

            Integer, -2^63 to 2^63 - 1.

        - double

            IEEE floating point number, 8 bytes.

        - enum

            Keyword with length up to 20 o.

        - text

            Text with length up to 500 o.

        - longtext

            Text with length up to 2^32 - 4 o.

        - datetime

            Timestamp.
            **Deprecated** as of Sympa 6.2.25b.3.
            Use `int(11)` (Unix time) instead.

        - mediumblob

            Binary data with length up to 2^24 - 3 o.

    - doc

        Description of the field.

    - primary

        If this is true, primary key consists of this field.

    - not\_null

        If this is true, Null value is not allowed.
        Note that fields included in primary key always don't allow Null value.

- db\_struct ()

    This function was OBSOLETED.

- not\_null ()

    _Function_.
    TBD.

- autoincrement ()

    _Function_.
    TBD.

- primary ()

    _Function_.
    TBD.

# SEE ALSO

[sympa\_database(5)](./sympa_database.5.md),
[Sympa::DatabaseManager](./Sympa-DatabaseManager.3.md).

# HISTORY

[Sympa::DatabaseDescription](./Sympa-DatabaseDescription.3.md) was introduced behind the veil on Sympa 6.1.
It began to be referred overtly as a part of Sympa Database Manager (SDM) on
Sympa 6.2.
