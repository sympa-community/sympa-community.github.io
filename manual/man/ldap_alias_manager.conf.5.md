---
title: 'ldap_alias_manager.conf(5)'
---

# NAME

ldap\_alias\_manager.conf - Configuration of LDAP alias management

# DESCRIPTION

`ldap_alias_manager.conf` is used by the [ldap\_alias\_manager(8)](./ldap_alias_manager.8.md);
it allows one to manage mail aliases in an LDAP directory.
To make sympa use the ldap\_alias\_manager.pl script, you should edit the
'alias\_manager' [sympa.conf(5)](./sympa.conf.5.md) parameter.

Format of `ldap_alias_manager.conf` is as following:

- Lines beginning with `#` and containing only spaces are ignored.
- Each line has the form "_parameter_ _value_".
_value_ may contain spaces but may not contain newlines.

## Parameters

- ldap\_host &lt;host>

    _Mandatory_. LDAP server host.

    Example:
      ldap\_host ldap.example.com

- ldap\_bind\_dn &lt;distinguished name>

    _Mandatory_. LDAP bind DN.

    Example:
      ldap\_bind\_dn cn=sympa,ou=services,dc=example,dc=com

- ldap\_bind\_pwd &lt;secret>

    _Mandatory_. LDAP bind password.

    Example:
      ldap\_bind\_pwd secret

- ldap\_base\_dn &lt;distinguished name>

    _Mandatory_. LDAP base DN.

    Example:
      ldap\_base\_dn ou=mail,dc=example,dc=com

- ldap\_mail\_attribute &lt;attribute name>

    _Mandatory_. LDAP mail attribute.

    Example:
      ldap\_mail\_attribute mail

- ldap\_ssl <0/1>

    _Mandatory_. Use TLS (SSL) for connection to LDAP server.

    Example:
      ldap\_ssl 0

- ldap\_ssl\_version &lt;sslv2 /sslv3 / tlsv1 / tlsv1\_1 / tlsv1\_2>

    _Mandatory_ if `ldap_ssl` is `1`. Protocol version of TLS.

    Example:
      ldap\_ssl\_version tlsv1

- ldap\_cachain &lt;file path>

    LDAP CA chain file

    Example:
      ldap\_cachain /etc/ldap/cert/cachain.pem

- queue\_transport &lt;name>

    _Mandatory_. Postfix transport parameter for queue

    Example:
      queue\_transport sympa

- bouncequeue\_transport &lt;name>

    _Mandatory_. Postfix transport parameter for bouncequeue

    Example:
      bouncequeue\_transport   sympabounce

# FILES

- `$SYSCONFDIR/ldap_alias_manager.conf`

    Configuration file.

# SEE ALSO

[ldap\_alias\_manager(8)](./ldap_alias_manager.8.md).
