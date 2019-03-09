---
title: 'Sympa::Scenario(3)'
---

# NAME

Sympa::Scenario - Authorization scenarios

# SYNOPSIS

    use Sympa::Scenario;
    
    my $scenario = Sympa::Scenario->new($list, 'send', name => 'private');
    my $result = $scenario->authz('md5', {sender => $sender});

# DESCRIPTION

[Sympa::Scenario](./Sympa-Scenario.3.md) provides feature of scenarios which perform authorization
on functions of Sympa software against users and clients.

## Methods

- new ( $that, $function, \[ name => $name \],
\[ dont\_reload\_scenario => 1 \] )

    _Constructor_.
    Creates a new [Sympa::Scenario](./Sympa-Scenario.3.md) instance.

    Parameters:

    - $that

        Context of scenario, list or domain
        (note that scenario does not have site context).

    - $function

        Specifies scenario function.

    - name => $name

        Specifies scenario name.
        If the name was not given, it is taken from list/domain configuration.
        See ["Scenarios"](#scenarios) for details.

    - dont\_reload\_scenario => 1

        If set, won't check if scenario files were updated.

    Returns:

    A new [Sympa::Scenario](./Sympa-Scenario.3.md) instance.

- authz ( $auth\_method, \\%context,  \[ debug => 1\] )

    _Instance method_.
    Return the action to perform for 1 sender
    using 1 auth method to perform 1 function.

    Parameters:

    - $auth\_method

        'smtp', 'md5', 'pgp', 'smime' or 'dkim'.
        Note that \`pgp\` has not been implemented.

    - \\%context

        A hashref containing information to evaluate scenario (scenario context).

    - debug => 1

        Adds keys in the returned hashref.

    Returns:

    A hashref containing following items.

    - {action}

        'do\_it', 'reject', 'request\_auth',
        'owner', 'editor', 'editorkey' or 'listmaster'.

    - {reason}

        Defined if {action} is 'reject' and in case `reject(reason='...')`:
        Key for template authorization\_reject.tt2.

    - {tt2}

        Defined if {action} is 'reject' and in case `reject(tt2='...')` or
        `reject('...tt2')`:
        Mail template name to be sent back to request sender.

    - {condition}

        The checked condition (defined if debug is set).

    - {auth\_method}

        The checked auth\_method (defined if debug is set).

- get\_current\_title ( )

    _Instance method_.
    Gets the title of the scenarioin the current language context.

- is\_purely\_closed ( )

    _Instance method_.
    Returns true value if the scenario obviously returns "reject" action.

- to\_string ( )

    _Instance method_.
    Returns source text of the scenario.

## Functions

- get\_scenarios ( $that, $function )

    _Function_.
    Gets all scenarios beloging to context $that and function $function.

- request\_action ( $that, $function, $auth\_method, \\%context,
\[ name => $name \], \[ dont\_reload\_scenario => 1 \], \[ debug => 1\] )

    _Function_. Obsoleted on Sympa 6.2.42. Use authz() method instead.

## Attributes

Instance of [Sympa::Scenario](./Sympa-Scenario.3.md) has these attributes:

- {context}

    Context given by new().

- {function}

    Name of function.

- {name}

    Scenario name.

- {file\_path}

    Full path of scenario file.

## Scenarios

A scenario file is named as _`function`_`.`_`name`_,
where _`function`_ is one of predefined function names, and
_`name`_ distinguishes policy.

If new() is called without `name` option, it is taken from configuration
parameter of context. Some functions don't have corresponding configuration
parameter and `name` options for them are mandatory.

# SEE ALSO

[sympa\_scenario(5)](./sympa_scenario.5.md).

# HISTORY

authz() method obsoleting request\_action() function introduced on
Sympa 6.2.41b.
