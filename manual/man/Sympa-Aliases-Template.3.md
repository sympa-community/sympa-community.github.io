---
title: 'Sympa::Aliases::Template(3)'
---

# NAME

Sympa::Aliases::Template -
Alias management: Aliases file based on template

# SYNOPSIS

    use Sympa::Aliases;
    my $aliases = Sympa::Aliases->new('Template',
        [ file => '/path/to/file' ] );
    $aliases->check('listname', 'domain');
    $aliases->add($list);
    $aliases->del($list);

# DESCRIPTION

[Sympa::Aliases::Template](./Sympa-Aliases-Template.3.md) manages list aliases based on template
`list_aliases.tt2`.

## Methods

- check ( $listname, $domain )

    See [Sympa::Aliases::CheckSMTP](./Sympa-Aliases-CheckSMTP.3.md).

- add ( $list )
- del ( $list )

    Adds or removes aliases of list $list.

    If constructor was called with `file` option, it will be used as aliases
    file and `sympa_newaliases` utility will not be executed.
    Otherwise, value of `sendmail_aliases` parameter will be used as aliases
    file and `sympa_newaliases` utility will be execeuted to update
    alias database.
    If `sendmail_aliases` parameter is set to `none`, aliases will never be
    updated.

## Configuration parameters

- return\_path\_suffix

    Suffix of list return address.

- sendmail\_aliases

    Path of the file that contains all list related aliases.

- tmpdir

    A directory temporary files are placed.

# FILES

- `$SYSCONFDIR/_domain name_/list_aliases.tt2`
- `$SYSCONFDIR/list_aliases.tt2`
- `$DEFAULTDIR/list_aliases.tt2`

    Template of aliases: Specific to a domain, global context and the default.

- `$SENDMAIL_ALIASES`

    Default location of aliases file.

- `$SBINDIR/sympa_newaliases`

    Auxiliary program to update alias database.

# SEE ALSO

[Sympa::Aliases](./Sympa-Aliases.3.md),
[Sympa::Aliases::CheckSMTP](./Sympa-Aliases-CheckSMTP.3.md),
[sympa\_newaliases(8)](./sympa_newaliases.8.md).

# HISTORY

`alias_manager.pl` to manage aliases using template appeared on
Sympa 3.1b.13.

[Sympa::Aliases::Template](./Sympa-Aliases-Template.3.md) module appeared on Sympa 6.2.23b,
and it obsoleted `alias_manager(8)`.
