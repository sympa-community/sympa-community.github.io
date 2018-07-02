---
title: 'Sympa::Config(3)'
---

# NAME

Sympa::Config - List configuration

# SYNOPSIS

    use base qw(Sympa::Config);
    
    sub _schema { {...} }

# DESCRIPTION

## Methods

- new ( $that, \[ config => $initial\_config \], \[ copy => 1 \],
\[ _key_ => _val_, ... \] )

    _Constructor_.
    Creates new instance of [Sympa::Config](./Sympa-Config.3.md) object.

    Parameters:

    - $that

        Context.  An instance of [Sympa::List](./Sympa-List.3.md) class, Robot or Site.

    - config => $initial\_config

        Initial configuration.

        - When the context object will be initially created,
        `undef` must be specified explicitly
        so that default parameter values will be completed.
        - When existing list will be instantiated and config will be loaded,
        `{}` (default) would be specified
        so that default parameter values except optional ones
        (with occurrence `'0-1'` or `'0-n'`) will be completed.
        - Otherwise, default parameter values are completed
        only when the new paragraph node will be added by submit().

        Note that initial configuration will never be sanitized.

    - copy => 1

        Uses deep copy of initial configuration (see ["config"](#config))
        instead of real reference.

- get ( $ppath )

    _Instance method_.
    Gets copy of current value of parameter.

    Parameter:

    - $ppath

        Parameter path,
        e.g.: `'owner.0.email'` specifies "email" parameter of
        the first "owner" paragraph;
        `'owner.0'` specifies the first "owner" paragraph;
        `'owner'` specifies the array of all "owner" paragraph.

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

- get\_schema ( \[ _options_, ... \] )

    _Instance method_.
    Get configuration schema as hashref.

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

- submit ( $new, $user, \\@errors, \[ no\_global\_validations => 1 \] )

    _Instance method_.
    Submits change and verifies it.
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

    - no\_global\_validations => 1

        If set, global validations are disabled.
        Global validations examine the entire configuration for semantic errors or
        requirements that can't be detected within a single paragraph.
        See also ["\_global\_validations"](#_global_validations).

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

### Methods child classes should implement

- \_schema ( )

    _Instance method_, _mandatory_.
    TBD.

- \_init\_schema\_item ( $pitem, $pnames, $subres,
\[ _key_ => _val_, ... \] )

    _Instance method_.
    TBD.

- \_global\_validations ( )

    _Class or instance method_.
    TBD.

- \_local\_validations ( )

    _Class or instance method_.
    TBD.

## Attribute

Instance of [Sympa::Config](./Sympa-Config.3.md) has following attribute.

- {context}

    Context, [Sympa::List](./Sympa-List.3.md) instance.

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

## Filters

TBD.

## Validations

TBD.

# SEE ALSO

[Sympa::ListDef](./Sympa-ListDef.3.md).

# HISTORY

[Sympa::Config](./Sympa-Config.3.md) appeared on Sympa 6.2.33b.2.
