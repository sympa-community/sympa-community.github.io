---
title: 'Sympa::Task(3)'
---

# NAME

Sympa::Task - Tasks of Sympa

# SYNOPSIS

    use Sympa::Task;
    
    $task = Sympa::Task->new($serialized, context => $list,
        model => 'remind', label => 'EXEC', date => 1234567890);
    
    $task = Sympa::Task->new(context => $list, model => 'remind');

# DESCRIPTION

## Methods

- new ( $serialized, context => $that, model => $model,
label => $label, date => $date )
- new ( context => $that, model => $model, \[ name => $name \],
\[ label => $label \], \[ date => $date \] )

    _Constructor_.
    Creates a new instance of [Sympa::Task](./Sympa-Task.3.md) class.

    The first style is usually used by task spool class
    (see also [Sympa::Spool::Task](./Sympa-Spool-Task.3.md)): `context`, `model`, `label` and `date`
    are given by metadata (file name).

    Parameters:

    - $serialized

        Serialized content of task file in spool.
        If omitted (the second style above), appropriate task file is read,
        parsed and used for serialized content.

    - context => $that

        Context of the task: List (instance of [Sympa::List](./Sympa-List.3.md)) or Site (`'*'`).

    - model => $model

        Task model.

    - name => $name

        Selector of task.
        If omitted, value of parameter of context object that corresponds to
        task model is used; if parameter value was not valid, constructor returns
        `undef`.

    - label => $label

        Label of task.
        If omitted, default label (in many cases, empty string) is used.

    - date => $date

        Unix time. creation date of label.
        If omitted, current time.

    Returns:

    New instance of [Sympa::Task](./Sympa-Task.3.md) class.

- dup ( )

    _Copy constructor_.
    Creates deep copy of instance.

- lines ( )

    _Instance method_.
    Gets an array of parsed information by each line of serialized content.

- to\_string ( )

    _Instance method_.
    Gets serialized content of the task. 

- get\_id ( )

    _Instasnce method_.
    Gets unique identifier of instance.

## Function

- get\_tasks ( $that, $model )

    _Function_.
    Gets all possible tasks for particular context.

    Parameters:

    - $that

        Context. Instance of [Sympa::List](./Sympa-List.3.md) or `'*'`.

    - $model

        Task model.

    Returns:

    An arrayref of possible tasks.

## Attributes

- {date}
- {model}
- {title}

    TBD.

# SEE ALSO

[task\_manager(8)](./task_manager.8.md).

[Sympa::Spool::Task](./Sympa-Spool-Task.3.md).

# HISTORY

[Task](https://metacpan.org/pod/Task) module appeared on Sympa 5.2b.1.
It was renamed to [Sympa::Task](./Sympa-Task.3.md) on Sympa 6.2a.41.

It was rewritten and split into Sympa::Task and [Sympa::Spool::Task](./Sympa-Spool-Task.3.md) on
Sympa 6.2.37b.2.
