# NAME

Sympa::List::Config - List configuration

# SYNOPSIS

     use Sympa::List::Config;
     my $config = Sympa::List::Config->new($list, {...});
    
     my $errors = []; 
     my $validity = $config->submit({...}, $user, $errors);
     $config->commit($errors);
     
     my ($value) = $config->get('owner.0.gecos');
     my @keys  = $config->keys('owner');

# DESCRIPTION

## Methods

- new ( $list, \[ config => $initial\_config \], \[ copy => 1 \],
\[ no\_family => 1 \] )

    _Constructor_.
    Creates new instance of [Sympa::List::Config](./Sympa::List::Config.3.md) object.

    Parameters:

    - $list

        Context.  An instance of [Sympa::List](./Sympa::List.3.md) class.

    - config => $initial\_config

        Initial configuration.

        - When the list will be initially created,
        `undef` must be specified explicitly
        so that default parameter values will be completed.
        - When exisiting list will be instantiated and config will be loaded,
        `{}` (default) would be specified
        so that default parameter values except optional ones
        (with occurrence `'0-1'` or `'0-n'`) will be completed.
        - Otherwise, default parameter values are completed
        only when the new paragraph node will be added by submit().

        Note that initial configuration will never be sanitized.

    - copy => 1

        Uses deep copy of initial configuration (see ["config"](#config))
        instead of real reference.

    - no\_family => 1

        Won't apply family constraint.
        By default, the constraint will be applied if the list is belonging to
        family.
        See also ["Family constraint"](#family-constraint).

- get ( $ppath )

    _Instance method_.
    Gets copy of current value of parameter.

    Parameter:

    - $ppath

        Parameter path,
        e.g.: `'owner.0.email'` specifys "email" parameter of
        the first "owner" paragraph;
        `'owner.0'` specifys the first "owner" paragraph;
        `'owner'` specifys the array of all "owner" paragraph.

    Returns:

    Value of parameter.
    If parameter or value does not exist, returns `undef` in scalar context
    and an empty list in array context.

- get\_change ( $ppath )

    _Instance method_.
    Gets copy of submitted change on parameter.

    Parameter:

    - $ppath

        Parameter path.
        See also get().

    Returns:

    If value won't be changed, returns empty list in array context
    and `undef` in scalar context.
    If value would be deleted, returns `undef`.

    Changes on the array are given by hashref
    with keys as affected indexes of the array.

- get\_changeset ( )

    _Instance method_.
    Gets all submitted changes.

    Note that returned value is the real reference to internal information.
    Any modifications might break it.

- get\_schema ( \[ $user \] )

    _Instance method_.
    Get configuration schema as hashref.
    See [Sympa::ListDef](./Sympa::ListDef.3.md) about structure of schema.

    Parameter:

    - $user

        Email address of a user.
        If specified, adds `'privilege'` attribute taken from [edit\_list.conf(5)](./edit_list.conf.5.md)
        for the user.

- keys ( \[ $pname \] )

    _Instance method_.
    Gets parameter keys in order defined by schema.

    Parameter:

    - $pname

        Full parameter name, e.g. `'owner'`.
        If omitted or false value,
        returns keys of top-level parameters.

    Returns:

    List of keys.
    If parameter does not exist or it does not have sub-parameters,
    i.e. it is not the paragraph, empty list.

- submit ( $new, $user, \\@errors )

    _Instance method_.
    Submits change and verifys it.
    Submission is done by:

    - Sanitizing changes:

        Omits unknown parameters,
        resolves parameter aliases,
        omits malformed change information,
        omits obsoleted parameters,
        omits changes on unwritable parameters,
        removes nodes under which required children nodes will be removed,
        resolves synonym of input values,
        canonicalizes inputs (see ["Filters"](#filters)),
        and omits identical changes.

    - Verifying changes:

        Omits removal of mandatory parameters,
        checks format of inputs,
        and performs additional validations (see ["Validations"](#validations)).

    Parameters:

    - $new

        Changes to be submitted, hashref.

    - $user

        Email of the user requesting submission.

    - \\@errors

        If errors occur, they will be pushed in this arrayref.
        Each element is arrayref `[ _type_, _error_, _info_ ]`:

        - _type_

            One of `'user'` (failure), `'intern'` (internal failure)
            and `'notice'` (successful notice).

        - _error_

            A keyword to determine error.

        - _info_

            Optional hashref with keys:
            `p_info` for schema item of parameter;
            `p_paths` for elements of parameter path;
            `value` for erroneous value (optional).

    Returns:

    If no changes found (or all changes were omitted), an empty string `''`.
    If any errors found in input, `'invalid'`.
    Otherwise, `'valid'`.

    In case any changes are submitted,
    changeset may be accessible by get\_change() or get\_changeset().

- commit ( \[ \\@errors \] )

    _Instance method_.
    Merges changes set by sbumit() into actual configuration.

    Parameter:

    - \\@errors

        Arrayref.
        See \\@errors in submit().

    Returns:

    None.
    Errors will be stored in arrayref.

## Attribute

Instance of [Sympa::List::Config](./Sympa::List::Config.3.md) has following attribute.

- {context}

    Context, [Sympa::List](./Sympa::List.3.md) instance.

## Structure of configuration

Configuration on the memory is represented by a hashref,
with its keys as node names and values as node values.

### Node types

Each node of configuration has one of following four types.
Some of them can include other type of nodes recursively.

- Set (multiple enumerated values)

    Arrayref.
    In the schema, defined with:

    - {occurrence}: `'0-n'` or `'1-n'`.
    - {format}: Arrayref.

    List of unique items not considering order.
    Items are scalars, and cannot be special values (scenario or task).
    The set cannot contain paragraphs, sets or arrays.

- Array (multiple values)

    Arrayref.
    In the schema, defined with:

    - {occurrence}: `'0-n'` or `'1-n'`.
    - {format}: Regexp or hashref.

    List of the same type of nodes in order.
    Type of all nodes can be one of paragraph,
    scalar or special value (scenario or task).
    The array cannot contain sets or arrays.

- Paragraph (structured value)

    Hashref.
    In the schema, defined with:

    - {occurrence}: If the node is an item of array, `'0-n'` or `'1-n'`.
    Otherwise, `'0-1'` or `'1'`.
    - {format}: Hashref.

    Compound node of one or more named nodes.
    Paragraph can contain any type of nodes, and each of their names and types
    are defined as member of {format} item in schema.

- Leaf (simple value)

    Scalar, or hashref for special value (scenario or task).
    In the schema, defined with:

    - {occurrence}: If the node is an item of array, `'0-n'` or `'1-n'`.
    Otherwise, `'0-1'` or `'1'`.
    - {format}: If the node is an item of array, regexp.
    Otherwise, regexp or arrayref.

    Scalar or special value (scenario or task).
    Leaf cannot contain any other nodes.

## Family constraint

The family (see [Sympa::Family](./Sympa::Family.3.md)) adds additional constraint to schema.
The family constraint

- restricts options for particular scalar parameters to the set of values
or single value,
- makes occurrence of them be required (`'1'` or `'1-n'`), and
- if the occurrence became `'1'`,
makes their privilege be unwritable (`'read'` if it was not `'hidden'`).

## Filters

TBD.

## Validations

TBD.

# SEE ALSO

[Sympa::List](./Sympa::List.3.md),
[Sympa::ListDef](./Sympa::ListDef.3.md).

# HISTORY

[Sympa::List::Config](./Sympa::List::Config.3.md) appeared on Sympa 6.2.17.
