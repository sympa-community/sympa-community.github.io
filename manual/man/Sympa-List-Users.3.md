---
title: 'Sympa::List::Users(3)'
---

# NAME

Sympa::List::Users - List users

# SYNOPSIS

     use Sympa::List::Users;
     my $config = Sympa::List::Users->new($list, {...});
    
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
    Creates new instance of [Sympa::List::Users](./Sympa-List-Users.3.md) object.

    Parameters:

    See also ["new" in Sympa::Config](./Sympa-Config.3.md#new).

    - $list

        Context.  An instance of [Sympa::List](./Sympa-List.3.md) class.

    - no\_family => 1

        Won't apply family constraint.
        By default, the constraint will be applied if the list is belonging to
        family.
        See also ["Family constraint" in Sympa::List::Config](./Sympa-List-Config.3.md#family-constraint).

- get\_schema ( \[ $user \] )

    _Instance method_.
    Get configuration schema as hashref.
    See [Sympa::ListDef](./Sympa-ListDef.3.md) about structure of schema.

    Parameter:

    - $user

        Email address of a user.
        If specified, adds `'privilege'` attribute taken from [edit\_list.conf(5)](./edit_list.conf.5.md)
        for the user.

## Attribute

See ["Attribute" in Sympa::Config](./Sympa-Config.3.md#attribute).

## Filters

TBD.

## Validations

TBD.

# SEE ALSO

[Sympa::Config](./Sympa-Config.3.md),
[Sympa::List::Config](./Sympa-List-Config.3.md),
[Sympa::ListDef](./Sympa-ListDef.3.md).

# HISTORY

[Sympa::List::Users](./Sympa-List-Users.3.md) appeared on Sympa 6.2.33b.2.
