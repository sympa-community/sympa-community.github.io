---
title: 'Sympa::Spool::Task(3)'
---

# NAME

Sympa::Spool::Task - Spool for tasks

# SYNOPSIS

    use Sympa::Spool::Task;
    my $spool = Sympa::Spool::Task->new;

    $spool->store($task);

    my ($task, $handle) = $spool->next;

# DESCRIPTION

[Sympa::Spool::Task](./Sympa-Spool-Task.3.md) implements the spool for tasks.

## Methods

See also ["Public methods" in Sympa::Spool](./Sympa-Spool.3.md#public-methods).

- next ( \[ no\_filter => 1 \], \[ no\_lock => 1 \] )

    Order is controlled by date element of file name.
    if `no_filter` is _not_ set,
    messages with date newer than current time are skipped.

    All necessary tasks are created and stored into spool in advance.

- quarantine ( $handle )

    Removes a task: The same as remove().
    This spool does not have `bad/` subdirectory.

## Context and metadata

See also ["Marshaling and unmarshaling metadata" in Sympa::Spool](./Sympa-Spool.3.md#marshaling-and-unmarshaling-metadata).

This class particularly gives following metadata:

- {date}

    Unix time when task will be executed at the next time.

- {label}
- {model}

    TBD.

# CONFIGURATION PARAMETERS

Following site configuration parameters in sympa.conf will be referred.

- queuetask

    Directory path of task spool.

# SEE ALSO

[task\_manager(8)](./task_manager.8.md), [Sympa::Spool](./Sympa-Spool.3.md), [Sympa::Task](./Sympa-Task.3.md).

# HISTORY

[Sympa::Spool::Task](./Sympa-Spool-Task.3.md) appeared on Sympa 6.2.37b.2.
