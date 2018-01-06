---
title: 'Sympa::Aliases::CheckSMTP(3)'
---

# NAME

Sympa::Aliases::CheckSMTP - Alias management: Check addresses using SMTP

# SYNOPSIS

    use Sympa::Aliases;
    my $aliases = Sympa::Aliases->new('CheckSMTP');
    $aliases->check('listname', 'domain');

# DESCRIPTION 

TBD.

## Methods

- check ($listname, $robot\_id)

    _Instance method_.
    Checks if the requested list exists already using SMTP 'RCPT TO'.

    Parameters:

    - $listname

        Name of the list.

    - $robot\_id

        List's robot.

    Returns:

    Instance of [Net::SMTP](https://metacpan.org/pod/Net::SMTP) class or false value.

## Configuration parameters

Following parameters in `sympa.conf` or `robot.conf` are referred by
this module.

- list\_check\_helo

    SMTP HELO (EHLO) parameter used for address verification.
    Default value is the host part of `list_check_smtp` parameter.

- list\_check\_smtp

    SMTP server to verify existence of the same addresses as the list.

- list\_check\_suffixes

    List of suffixes used for list addresses.

# SEE ALSO

[Sympa::Aliases](./Sympa-Aliases.3.md).

# HISTORY

The feature which allows Sympa to check listname on SMTP server
before list creation, contributed by Sergiy Zhuk, appeared on Sympa 3.3.

`list_check_helo` parameter was added by S. Ikeda on Sympa 6.1.5.

[Sympa::Aliases::CheckSMTP](./Sympa-Aliases-CheckSMTP.3.md) as a separate module appeared on Sympa 6.2.23b.
