# NAME

sympa\_test\_ldap, sympa\_test\_ldap.pl - Testing LDAP connection for Sympa

# SYNOPSIS

    sympa_test_ldap.pl --filter=string --host=string --suffix=string
    [ --attrs=[ string,...|* ] ]
    [ --bind_dn=string [ --bind_password=string ] ]
    [ --port=string ] [ --scope=base|one|sub ]
    [ --use_tls=starttls|ldaps|none
      [ --ca_file=string ] [ --ca_path=string ]
      [ --ca_verify=none|optional|require ]
      [ --ssl_cert=string ] [ --ssl_ciphers=string ] [ --ssl_key=string ]
      [ --ssl_version=sslv2|sslv3|tlsv1|tlsv1_1|tlsv1_2 ] ]

    sympa_test_ldap.pl --help

    sympa_test_ldap.pl --version

# DESCRIPTION

sympa\_test\_ldap.pl tests LDAP connection and search operation using LDAP
driver of Sympa.

# SEE ALSO

[Sympa::DatabaseDriver::LDAP](./Sympa-DatabaseDriver-LDAP.3.md).

# HISTORY

testldap.pl was renamed to sympa\_test\_ldap.pl on Sympa 6.2.

`--use_ssl` and `--use_start_tls` options were obsoleted by Sympa 6.2.15.
`--use_tls` option would be used instead.
