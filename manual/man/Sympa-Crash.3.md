---
title: 'Sympa::Crash(3)'
---

# NAME

Sympa::Crash - Show traceback on critical error

# SYNOPSIS

    use Sympa::Crash;
    
    # Registering custom hook.
    use Sympa::Crash Hook => \&myhook;

# DESCRIPTION

Once [Sympa::Crash](./Sympa-Crash.3.md) is loaded, crash by runtime error will be reported
via log and traceback will be shown in standard error.

If optional `Hook` parameter is given, it will be executed instead.

## Function

- register\_handler ( )

    Sometimes other modules overwrites error handler.
    If that is the case, executing this function will register handler again.

# HISTORY

Sympa::Crash appeared on Sympa 6.2.
