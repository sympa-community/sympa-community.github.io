---
title: 'Sympa::DatabaseManager(3)'
---

# NAME

Sympa::DatabaseManager - Managing schema of Sympa core database

# SYNOPSIS

    use Sympa::DatabaseManager;
    
    $sdm = Sympa::DatabaseManager->instance
        or die 'Cannot connect to database';
    $sth = $sdm->do_prepared_query('SELECT FROM ...', ...)
        or die 'Cannot execute query';
    Sympa::DatabaseManager->disconnect;

    Sympa::DatabaseManager::probe_db() or die 'Database is not up-to-date';

# DESCRIPTION

[Sympa::DatabaseManager](./Sympa-DatabaseManager.3.md) provides functions to manage schema of Sympa core
database.

## Methods and functions

- instance ( )

    _Constructor_.
    Gets singleton instance of Sympa::Database class managing Sympa core database.

- disconnect ( )

    _Class method_.
    Disconnects from core database.

- probe\_db ( )

    _Function_.
    If possible, probes database structure and updates it.

# SEE ALSO

[Sympa::Database](./Sympa-Database.3.md), [Sympa::DatabaseDescription](./Sympa-DatabaseDescription.3.md), [Sympa::DatabaseDriver](./Sympa-DatabaseDriver.3.md).

# HISTORY

Sympa Database Manager (SDM) appeared on Sympa 6.2.
