# NAME

Sympa::Spindle - Base class of subclasses to define Sympa workflows

# SYNOPSIS

     package Sympa::Spindle::FOO;
     use base qw(Sympa::Spindle);

     use constant _distaff => 'Sympa::Spool::BAR';
     
     sub _twist {
         my $self = shift;
         my $object = shift;
     
         # Process object...
    
         return 1;                        # If succeeded.
         return 0;                        # If skipped.
         return undef;                    # If failed.
         return ['Sympa::Spindle::BAZ'];  # Splicing to the other class(es).
     }

     1;

# DESCRIPTION

[Sympa::Spindle](./Sympa-Spindle.3.md) is the base class of subclasses to define particular
workflows of Sympa.

A spindle class is a set of features to process objects.
If spin() method is called, it retrieves each object from source spool,
processes it, at last passes altered object to appropriate destination
(another spool or mailer), and removes it as necessity.
Processing repeats until source spool is empty.

## Public methods

- new ( \[ key => value, ... \] )

    _Constructor_.
    Creates new instance of [Sympa::Spindle](./Sympa-Spindle.3.md).

- spin ( )

    _Instance method_.
    Fetches an object and handle locking it from source spool, processes them
    calling \_twist() and repeats.
    If source spool no longer gives content, returns the number of processed
    objects.

- add\_stash ( =_parameters_... )

    _Instance method_.
    Adds arrayref of parameters to a storage for general-purpose.

## Properties

Instance of [Sympa::Spindle](./Sympa-Spindle.3.md) may have following properties.

- {distaff}

    Instance of source spool class \_distaff() method returns.

- {finish}

    _Read/write_.
    At first this property is false.
    Once it is set, spin() finishes processing safely.

- Spools

    Instances of spool classes \_spools() method returns.

- {start\_time}

    Unix time in floating point number when processing of the latest message by
    spin() began.
    Introduced by Sympa 6.2.13.

- {stash}

    A reference to array of added data by add\_stash().

## Methods subclass should implement

- \_distaff ( )

    _Class method_, _mandatory_ if you want to implement spin().
    Returns the name of source spool class.
    source spool class must implement new() and next().

- \_init ( $state )

    _Instance method_.
    Additional processing
    when the spindle class is instantiated ($state is 0), before spin() processes
    next object in source spool ($state is 1) or after it processed object
    ($state is 2).

    If it returns false value, new() will return `undef` (when $state is 0)
    or spin() will terminate processing (when $state is 1).
    By default it always returns `1`.

- \_on\_garbage ( $handle )

    _Instance method_, _overridable_.
    Executes process when object could not be deserialized (new() method of object
    failed).
    By default, quarantines object calling quearantine() method of source spool.

- \_on\_failure ( $message, $handle )

    _Instance method_, _overridable_.
    Executes process when processing of $message failed (\_twist() returned
    `undef`).
    By default, quarantines object calling quearantine() method of source spool.

- \_on\_skip ( $message, $handle )

    _Instance method_, _overridable_.
    Executes process when $message was skipped (\_twist() returned `0`).
    By default, simply unlocks object calling close() method of $handle.

- \_on\_success ( $message, $handle )

    _Instance method_, _overridable_.
    Executes process when processing of $message succeeded (\_twist() returned true
    value).
    By default, removes object calling remove() method of source spool.

- \_spools ( )

    _Class method_.
    If implemented, returns hashref with names of spool classes related to the
    spindle as values.

- \_twist ( $message )

    _Instance method_, _mandatory_.
    Processes an object: Typically, modifys object or creates another object and
    stores it into appropriate spool.

    Parameter:

    - $message

        An object.

    Returns:

    Status of processing:
    True value on success; `0` if processing skipped; `undef` on failure.

    As of Sympa 6.2.13, \_twist() may also return the reference to array including
    name(s) of other classes:
    In this case spin() and twist() will call \_twist() method of given classes in
    order (not coercing spindle object into them) and uses retruned false value
    at first or true value at last.

# SEE ALSO

[Sympa::Internals::Workflow](./Sympa-Internals-Workflow.3.md),
[Sympa::Spool](./Sympa-Spool.3.md).

# HISTORY

[Sympa::Spindle](./Sympa-Spindle.3.md) appeared on Sympa 6.2.10.
