# NAME

Sympa::Spool - Base class of spool classes

# SYNOPSIS

    package Sympa::Spool::FOO;
    
    use base qw(Sympa::Spool);
    
    sub _directories {
        return {
            directory     => '/path/to/spool',
            bad_directory => '/path/to/spool/bad',
        };
    }
    use constant _generator      => 'Sympa::Message';
    use constant _marshal_format => '%s@%s.%ld.%ld,%d';
    use constant _marshal_keys   => [qw(localpart domainpart date PID RAND)];
    use constant _marshal_regexp =>
        qr{\A([^\s\@]+)(?:\@([\w\.\-]+))?\.(\d+)\.(\w+)(?:,.*)?\z};
    
    1;

# DESCRIPTION

This module is the base class for spool subclasses of Sympa.

## Public methods

- new ( \[ options... \] )

    _Constructor_.
    Creates new instance of the class.

- next ( \[ no\_lock => 1 \] )

    _Instance method_.
    Gets next message to process, order is controlled by name of spool file and
    so on.
    Message will be locked to prevent multiple proccessing of a single message.

    Parameters:

    - no\_lock => 1

        Won't lock messages.

    Returns:

    Two-elements list of message instance and filehandle locking
    a message.
    If parsing message fails, list of `undef` and filehandle.
    If no more message found, empty array.

    If `no_lock` is set,
    true scalar value will be returned in place of filehandle.

- quarantine ( $handle )

    _Instance method_.
    Quarantines a message.
    On filesystem spool,
    message will be moved into `{bad_directory}` of the spool using rename().

    Parameter:

    - $handle

        Filehandle, [Sympa::LockedFile](./Sympa::LockedFile.3.md) instance, locking message.

    Returns:

    True value if message could be quarantined.
    Otherwise false value.

    If $handle was not a filehandle, this method will die.

- remove ( $handle )

    _Instance method_.
    Removes a message.

    Parameter:

    - $handle

        Filehandle, [Sympa::LockedFile](./Sympa::LockedFile.3.md) instance, locking message.

    Returns:

    True value if message could be removed.
    Otherwise false value.

    If $handle was not a filehandle, this method will die.

- size ( )

    _Instance method_.
    Gets the number of messages the spool contains.

    Parameters:

    None.

    Returns:

    Number of messages.

    Note:
    This method returns the number of messages \_load() returns,
    not applying \_filter().

- store ( $message, \[ original => $original \] )

    _Instance method_.
    Stores the message into spool.

    Parameters:

    - $message

        Message to be stored.

    - original => $original

        If true value is specified and $message was decrypted,
        Stores original encrypted form.

    Returns:

    If storing succeeded, marshalled metadata (file name) of the message.
    Otherwise `undef`.

## Properties

Instance of [Sympa::Spool](./Sympa::Spool.3.md) may have following properties.

- Directories

    Directories \_directories() method returns:
    `{directory}`, `{bad_directory}` and so on.

## Low level functions

- build\_glob\_pattern ( $marshal\_format, $marshal\_keys,
\[ key => value, ... \] )

    _Function_.
    Builds a glob pattern from parameters and returns it.
    If built pattern is empty or contains only punctuations,
    i.e. `[^0-9A-Za-z\x80-\xFF]`, will return `undef`.

- split\_listname ( $robot, $localpart )

    _Function_.
    Split local part of e-mail to list name and type.
    Returns an array `(name, type)`.
    Note that the list with returned name may or may not exist.

    If local part looks like listmaster or sympa address, name is `undef` and
    type is either `'listmaster'` or `'sympa'`.
    Otherwise, type is either `'editor'`, `'owner'`, `'return_path'`,
    `'subscribe'`, `'unsubscribe'`, `'UNKNOWN'` or `undef`.

    Note:
    For `-request` and `-owner` suffix, this function returns
    `owner` and `return_path` types, respectively.

- store\_spool ( $spool\_dir, $message, $marshal\_format, $marshal\_keys,
\[ key => value, ... \] )

    _Function_.
    Store $message into directory $spool\_dir as a file with name as
    marshalled metadata using $marshal\_format and $marshal\_keys.

- unmarshal\_metadata ( $spool\_dir, $marshalled,
$marshal\_regexp, $marshal\_keys )

    _Function_.
    Unmarshals metadata.
    Returns hashref with keys in arrayref $marshal\_keys
    and values with substrings captured by regexp $marshal\_regexp.
    If $marshalled did not match against $marshal\_regexp,
    returns `undef`.

    The keys `localpart` and `domainpart` are special.
    Following keys are derived from them:
    `context`, `listname`, `listtype`, `priority`.

- marshal\_metadata ( $message, $marshal\_format, $marshal\_keys )

    _Function_.
    Marshals metadata.
    Returns formatted string by sprintf() using $marshal\_format
    and metadatas indexed by keys in arrayref $marshal\_keys.

    If key is uppercase, it means auto-generated value:
    `'AUTHKEY'`, `'KEYAUTH'`, `'PID'`, `'RAND'`, `'TIME'`.
    Otherwise it means metadata or property of $message.

    sprintf() is executed under `C` locale:
    Full stop (`.`) is always used for decimal point in floating point number.

## Methods subclass should implement

- \_create ( )

    _Instance method_, _overridable_.
    Creates spool.
    By default, creates directories returned by \_directories().

- \_directories ( \[ options, ... \] )

    _Class or instance method_, _mandatory for filesystem spool_.
    Returns hashref with directory paths related to the spool as values.
    It must have keys at least `directory` and
    (if you wish to implement quarantine() method) `bad_directory`.

- \_filter ( $metadata )

    _Instance method_, _overridable_.
    If it returned false value, processing of $metadata by next() will be skipped.
    By default, always returns true value.

    This method may modify unmarshalled metadata \_and\_ deserialized messages
    include it.

- \_filter\_pre ( $metadata )

    _Instance method_, _overridable_.
    If it returned false value, processing of $metadata by store() will be
    skipped.
    By default, always returns true value.

    This method may modify marshalled metadata \_but\_ stored messages are not
    affected.

- \_generator ( )

    _Class or instance method_, _mandatory_.
    Returns name of the class to serialize and deserialize messages in the spool.
    If spool subclass is the collection (see \_is\_collection),
    generator class must implement new().
    Otherwise,
    generator class must implement dup(), new() and to\_string().

- \_glob\_pattern ( )

    _Instance method_.
    If implemented and returns non-empty string,
    glob() is used to search entries in the spool.
    Otherwise readdir() is used for filesystem spool to get all entries.

- \_init ( $state )

    _Instance method_.
    Additional processing when \_load() returns no contents ($state is 1) or
    when the spool class is instantiated ($state is 0).

- \_is\_collection ( )

    _Instance method_, _overridable_.
    If the class is collection of spool class, returns true value.
    By default returns false value.

    Collection class does not have store() method.
    Its content is the set of spool instances.

- \_load ( )

    _Instance method_, _overridable_.
    Loads sorted content of spool.
    Returns arrayref of marshalled metadatas.
    By default, returns content of `{directory}` directory sorted by file name.

- \_marshal\_format ( )
- \_marshal\_keys ( )
- \_marshal\_regexp ( )

    _Instance methods_, _mandatory for filesystem spool_.
    \_marshal\_format() and \_marshal\_keys() are used to marshal metadata.
    \_marshal\_keys() and \_marshal\_regexp() are used to unmarshal metadata.
    See also marshal\_metadata() and unmarshal\_metadata().

- \_store\_key ( )

    _Instance method_.
    If implemented and returns non-empty string,
    store() returns an item in metadata specified by this method.
    By default store() returns marshalled metadata
    (file name on filesystem spool).

## Marshaling and unmarshaling metadata

Spool class gives generator class the **metadata** to instantiate it.
On spool based on filesystem, it is typically encoded into file names.
For example a file name in incoming spool ([Sympa::Spool::Incoming](./Sympa::Spool::Incoming.3.md))

    listname-owner@domain.name.143599229.12345

encodes the metadata

    localpart  => 'listname-owner',
    listname   => 'listname',
    listtype   => 'return_path',
    domainpart => 'domain.name',
    date       => 143599229,

Metadata always includes information of **context**: List, Robot or
Site.  For example:

\- Message in incoming spool bound for <listname@domain.name>:

    context    => Sympa::List <listname@domain.name>,

\- Command message in incoming spool bound for <sympa@domain.name>:

    context    => 'domain.name',

\- Message sent from Sympa to super-listmaster(s):

    context    => '*'

Context is determined when the generator class is instantiated, and
generally never changed through lifetime of instance.
Thus, constructor of generator class should take context object as an
argument.

# CONFIGURATION PARAMETERS

Following site configuration parameters in sympa.conf will be referred.

- default\_list\_priority
- email
- owner\_priority
- list\_check\_suffix
- listmaster\_email
- request\_priority
- return\_path\_suffix
- sympa\_priority

    Used to extract metadata from marshalled data (file name).

- umask

    The umask to make directories of spool.

# SEE ALSO

[Sympa::Message](./Sympa::Message.3.md), especially [Serialization](./Sympa::Message.3.md#serialization).

# HISTORY

[Sympa::Spool](./Sympa::Spool.3.md) appeared on Sympa 6.2.
It as the base class appeared on Sympa 6.2.6.

build\_glob\_pattern(), size(), \_glob\_pattern() and \_store\_key()
were introduced on Sympa 6.2.8.
\_filter\_pre() was introduced on Sympa 6.2.10.
