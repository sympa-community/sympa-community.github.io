---
title: 'Sympa::SharedDocument(3)'
---

# NAME

Sympa::SharedDocument - Shared document repository and its nodes

# SYNOPSIS

    use Sympa::SharedDocument;
    
    $shared = Sympa::SharedDocument->new($list, $path);
    
    %access = $shared->get_privileges('read', $email, 'md5', {...});
    @children = $shared->get_children;
    $parent = $shared->{parent};

# DESCRIPTION

[Sympa::SharedDocument](./Sympa-SharedDocument.3.md) implements shared document repository of lists.

## Methods

- new ( $list, \[ $path, \[ allow\_empty => 1 \] \] )

    _Constructor_.
    Creates new instance.

    Parameters:

    - $list

        A [Sympa::List](./Sympa-List.3.md) instance.

    - $path

        String to determine path or arrayref of path components.
        The path is relative to repository root.

    - allow\_empty => 1

        Don't omit files with zero size.

    Returns:

    If $path is empty or not specified, returns new instance of repository root;
    {status} attribute will be set.
    If $path is not empty and the path exists, returns new instance of node.
    Otherwise returns false value.

- as\_hashref ( )

    _Instance method_.
    Casts the instance to hashref.

    Parameters:

    None.

    Returns:

    A hashref including attributes of instance (see ["Attributes"](#attributes))
    and following special items:

    - {ancestors}

        Arrayref of hashrefs including some attributes of all ancestor nodes.

    - {context}

        Hashref including name and host of the list.

    - {date}

        Localized form of {date\_epoch}.

    - {parent}

        Hashref including attributes of parent node recursively.

    - {paths\_d}

        Same as {paths} but, if the node is a directory, includes additional empty
        component at the end.
        This is useful when the path created by join() should be followed by
        additional "/" character.

- count\_children ( )

    _Instance method_.
    Returns number of child nodes.

- count\_moderated\_descendants ( )

    _Instance method_.
    Returns number of nodes waiting for moderation.

- create\_child ( $name, owner => $email, scenario => $scenario,
type => $type, \[ content => $content \] )

    _Instance method_.
    Creates child node and returns it.
    TBD.

- get\_children ( \[ moderate => boolean \], \[ name => $name \],
\[ order\_by => $order \], \[ owner => $email \], \[ allow\_empty => 1 \] )

    _Instance method_.
    Gets child nodes.

    Parameters:

    - moderate => boolean
    - name => $name
    - owner => $email

        Filters results.

    - order\_by => $order

        Sorts results.
        $order may be one of
        `'order_by_doc'` (by name of nodes),
        `'order_by_author'` (by owner),
        `'order_by_size'` (by size),
        `'order_by_date'` (by modification time).
        Default is ordering by names.

    - allow\_empty => 1

        Don't omit nodes with zero size.

    Returns:

    (Possiblly empty) list of child nodes.

- get\_moderated\_descendants ( )

    _Instance method_.
    Returns the list of nodes waiting for moderation.

    Parameters:

    None.

    Returns:

    In array context, a list of nodes.
    In scalar context, an arrayref of them.

- get\_privileges ( mode => $mode, sender => $sender,
auth\_method => $auth\_method, scenario\_context => $scenario\_context )

    _Instance method_.
    Gets privileges of a user on the node.

    TBD.

- get\_size ( )

    _Instance method_.
    Gets total size under current node.

- install ( )

    _Instance method_.
    Approves (install) file if it was held for moderation.

    Returns:

    True value.
    If installation failed, returns false value and sets $ERRNO ($!).

- rename ( $new\_name )

    _Instance method_.
    Renames file or directory.

    Parameters:

    - $new\_name

        The name to be renamed to.

    Returns:

    True value.
    If renaming failed, returns false value and sets $ERRNO ($!).

- rmdir ( )

    _instalce method_.
    Removes directory from repository.
    Directory must be empty.

    Returns:

    True value.
    If removal failed, returns false value and sets $ERRNO ($!).

- save\_description ( )

    _Instance method_.
    Creates or updates property description of the node.

- unlink ( )

    _instalce method_.
    Removes file from repository.

    Returns:

    True value.
    If removal failed, returns false value and sets $ERRNO ($!).

- get\_id ( )

    _Instance method_.
    Returns unique identifier of instance.

### Methods for repository root

- create ( )

    _Instance method_.
    Creates document repository on physical filesystem.

- delete ( )

    _Instance method_.
    Deletes document repository.

- restore ( )

    _Instance method_.
    Restores deleted document repository.

## Functions

- valid\_name ( $new\_name )

    _Function_.
    Check if the name is allowed for directory and file.

    Note:
    This should be used with name of newly created node.
    Existing files and directories may have the name not allowed by this function.

## Attributes

Instance of [Sympa::SharedDocument](./Sympa-SharedDocument.3.md) may have following attributes.

- {context}

    _Mandatory_.
    Instance of [Sympa::List](./Sympa-List.3.md) class the shared document repository belongs to.

- {date\_epoch}

    _Mandatory_.
    Modification time of node in Unix time.

- {file\_extension}

    File extension if any.

- {fs\_name}

    _Mandatory_.
    Name of node on physical filesystem,
    i.e. the last part of {fs\_path}.

- {fs\_path}

    _Mandatory_.
    Full path of node on physical filesystem.

- {html}

    Only in HTML file.
    True value will be set.

- {icon}

    URL to icon.

- {label}

    Only in bookmark file.
    Label to be shown in hyperlink.

- {mime\_type}

    Only in regular file.
    MIME content type of the file if it is known.

- {moderate}

    Set if node is held for moderation.

- {name}

    _Mandatory_.
    Name of node accessible by users,
    i.e. the last item of {paths}.

- {owner}

    Owner (author) of node,
    given by property description.

- {parent}

    Parent node if any.  [Sympa::SharedDocument](./Sympa-SharedDocument.3.md) instance.

- {paths}

    _Mandatory_.
    Arrayref to all path components of node accessible by users.

- {scenario}{read}
- {scenario}{edit}

    Scenario names to define privileges.
    These may be given by property description.

- {serial\_desc}

    Modification time of property description in Unix time.
    Available if property description exists.

- {size}

    Size of file.

- {status}

    _Only in repository root_.
    Status of repository:
    `'exist'`, `'deleted'` or `'none'`.

- {title}

    Description of node,
    given by property description.

- {type}

    _Mandatory_.
    Type of node.
    `'root'` (the root of repository), `'directory'` (directory), `'url'`
    (bookmark file) or `'file'` (other file).

- {url}

    Only in bookmark file.
    URL to be linked.

# FILES

- _list home_/shared/

    Root of repository.

- _... path_/_name_

    Directory or file.

- _... path_/._name_.moderate

    Moderated directory or file.

- _... path_/_name_/.desc
- _... path_/.desc._name_
- _... path_/.desc.._name_.moderate

    Property description of directories or files, not moderated or moderated.

Note:
The path components ("_name_" above) are encoded to the format suitable to
physical filesystem.
Such conversion will be hidden behind object methods.

# SEE ALSO

[Sympa::List](./Sympa-List.3.md),
["qdecode\_filename" in Sympa::Tools::Text](./Sympa-Tools-Text.3.md#qdecode_filename),
["qencode\_filename" in Sympa::Tools::Text](./Sympa-Tools-Text.3.md#qencode_filename).

# HISTORY

[SharedDocument](https://metacpan.org/pod/SharedDocument) module appeared on Sympa 5.2b.2.

Rewritten [Sympa::SharedDocument](./Sympa-SharedDocument.3.md) began to provide OO interface on
Sympa 6.2.17.
