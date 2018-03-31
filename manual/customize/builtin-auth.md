---
title: 'Built-in authentication'
up: ../customize.md#customizing-web-interface
---

Built-in authentication
================================

See also "[Authentication on web interface](authentication-web.md)".

The build-in
[authentication mechanism](authentication-web.md#authentication-mechanisms)
provides authentication for user of web interface (WWSympa) using their
e-mail addresses and passwords.  It is enabled by default.

Requirements
------------

  * To use bcrypt hash (see below), you have to install the
    [Crypt-Eksblowfish](http://search.cpan.org/dist/Crypt-Eksblowfish/)
    Perl module.
    It is recommended.

Sympa configuration
-------------------

  * Make sure that [`user_table`](../man/auth.conf.5.md#ldap-paragraph)
    paragraph in [`auth.conf`](../man/auth.conf.5.md) exists.  It enables
    default authentication.

    If this paragraph is omitted, built-in authentication is disabled.

In [`sympa.conf`](../layout.md#config), following parameters are available:

  * [`password_case`](../man/sympa.conf.5.md#password_case)

    If set to be `insensitive`, storing and checking password will be
    case-insensitive.

    Note that, once you set this parameter, you should not change it.
    Or all user passwords may be invalidated.

  * [`password_hash`](../man/sympa.conf.5.md#password_hash)

    Information of users are stored in database table
    [`user_table`](../man/sympa_database.5.md#user_table).
    This parameter specifies a method to generate password hash.  Currently
    these values are possible:

      - `md5`

         Uses MD5 digest algorithm.

      - `bcrypt`

         Uses bcrypt hash function.

    ----
    Note:

      * The bcrypt hash was introduced on Sympa 6.2.26.  See also
        "[History of password storage format of Sympa](#history-of-password-storage-format-of-sympa)"
        below.

    ----

  * [`password_hash_update`](../man/sympa.conf.5.md#password_hash_update)

    If this parameter is set to `1` any supported password hash type (currently `md5` or `bcrypt`) 
    that does not match the current setting of `password_hash` is updated to
    the current hash type upon successful login.
    
    For instance, if `password_hash_update` is `1` and `password_hash` has been set to
    `bcrypt`, Sympa  will update an MD5 password hash to a bcrypt hash on successful login.
    
    If this parameter is set to `0`, only the hash type specified by the `password_hash` parameter is available.
    All previously set passwords are effectively invalidated.
    
    The default value is `1`, which is intended to support a graceful transition to a
    new hash type over a period of time.

  * [`bcrypt_cost`](../man/sympa.conf.5.md#bcrypt_cost)

    This parameter controls cost of hash stretching by bcrypt hash function.
    Available value is a positive integer less than or equal to `99`.
    `12` is used by default.

Upgrading password storage on earlier version
---------------------------------------------

If you are planning to upgrade Sympa 5.4.x or earlier, you should upgrade
password storage.  See
"[Upgrading from Sympa 5.4.x or earlier](../upgrade/notes.md#upgrading-from-sympa-54x-or-earlier)"
in "Upgrading notes".

History of password storage format of Sympa
-------------------------------------------

On very early versions of Sympa, passwords of users were stored in database
table as plain text.

As of Sympa 3.1 (2001), passwords were stored as encrypted form with
[RC4](https://tools.ietf.org/html/draft-kaukonen-cipher-arcfour-03)
reversible encryption algorithm.

Sympa 6.0 (2009) adopted [MD5](https://tools.ietf.org/html/rfc6151)
digest algorithm for newly created password.

Sympa 6.2.26 (2018) adopted
[bcrypt](https://www.usenix.org/legacy/publications/library/proceedings/usenix99/provos/provos_html/backup.html)
hash function using randomly generated salt.

