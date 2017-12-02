# NAME

Sympa::Aliases::External -
Alias management: Updating aliases by external program

# SYNOPSIS

    use Sympa::Aliases;
    
    my $aliases = Sympa::Aliases->new('/path/to/program',
        [ file => 'file' ] );
    # or,
    my $aliases = Sympa::Aliases->new('External',
        program => '/path/to/program', [ file => 'file' ] );
    
    $aliases->check('listname', 'domain');
    $aliases->add($list);
    $aliases->del($list);

# DESCRIPTION 

[Sympa::Aliases::External](./Sympa-Aliases-External.3.md) manages list aliases using external program.

## Methods

- check ( $listname, $domain )

    See [Sympa::Aliases::CheckSMTP](./Sympa-Aliases-CheckSMTP.3.md).

- add ( $list )
- del ( $list )

    Invokes program with command line arguments:

        /path/to/program add | del listname domain [ file ]

    If processing succeed, program should exit with status 0.
    Otherwise it may exit with non-zero status (see also ["Constants"](#constants)).

## Constants

### Exit status

- ERR\_CONFIG

    Configuration file has errors.

- ERR\_PARAMETER

    Incorrect call to program.

- ERR\_WRITE\_ALIAS

    Unable to append to alias.

- ERR\_NEWALIASES

    Unable to run newaliases command.

- ERR\_READ\_ALIAS

    Unable to read existing aliases.

- ERR\_CREATE\_TEMP

    Could not create temporay file.

- ERR\_ALIAS\_EXISTS

    Some of list aliases already exist.

- ERR\_LOCK

    Can not lock resource.

- ERR\_ALIASES\_EMPTY

    The parser returned empty aliases.

# SEE ALSO

[Sympa::Aliases](./Sympa-Aliases.3.md),
[Sympa::Aliases::CheckSMTP](./Sympa-Aliases-CheckSMTP.3.md).

# HISTORY

[Sympa::Aliases::External](./Sympa-Aliases-External.3.md) module appeared on Sympa 6.2.23b.
