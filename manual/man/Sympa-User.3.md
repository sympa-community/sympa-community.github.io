---
title: 'Sympa::User(3)'
---

# NAME

Sympa::User - All Users Identified by Sympa

# DESCRIPTION

## CONSTRUCTOR

- new ( EMAIL, \[ KEY => VAL, ... \] )

    Create new Sympa::User object.

## METHODS

- expire

    Remove user information from user\_table.

- get\_id

    Get unique identifier of object.

- moveto

    Change email of user.

- save

    Save user information to user\_table.

### ACCESSORS

- &lt;attribute>
- &lt;attribute>`( VALUE )`

    _Getters/Setters_.
    Get or set user attributes.
    For example `$user->gecos` returns "gecos" parameter of the user,
    and `$user->gecos("foo")` also changes it.
    Basic user profile "email" have only getter,
    so it is read-only.

## FUNCTIONS

- get\_users ( ... )

    Not yet implemented.

- password\_fingerprint ( )

    Returns the password finger print.

## OLD STYLE FUNCTIONS

- add\_global\_user
- delete\_global\_user
- is\_global\_user
- get\_global\_user
- get\_all\_global\_user

    _Obsoleted_.

- update\_global\_user

## Miscellaneous

- clean\_user ( USER\_OR\_HASH )
- clean\_users ( ARRAYREF\_OF\_USERS\_OR\_HASHES )

    _Function_.
    Warn if the argument is not a Sympa::User object.
    Return Sympa::User object, if any.

    _TENTATIVE_.
    These functions will be used during transition between old and object-oriented
    styles.  At last modifications have been done, they shall be removed.
