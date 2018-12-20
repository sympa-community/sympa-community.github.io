---
title: 'Sympa::Spindle::ProcessTask(3)'
---

# NAME

Sympa::Spindle::ProcessTask - Workflow of task processing

# SYNOPSIS

    use Sympa::Spindle::ProcessTask;
    
    my $spindle = Sympa::Spindle::ProcessTask->new;
    $spindle->spin;

# DESCRIPTION

[Sympa::Spindle::ProcessTask](./Sympa-Spindle-ProcessTask.3.md) defines workflow to process tasks in
task spool.

When spin() method is invoked, tasks kept in task spool are processed.
Then, all global and list tasks are created as necessity.

# SEE ALSO

[task\_manager(8)](./task_manager.8.md), [Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spool::Task](./Sympa-Spool-Task.3.md), [Sympa::Task](./Sympa-Task.3.md).

# HISTORY

[Sympa::Spindle::ProcessTask](./Sympa-Spindle-ProcessTask.3.md) appeared on Sympa 6.2.37b.2.
