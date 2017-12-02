# NAME

Sympa::LockedFile - Filehandle with locking

# SYNOPSIS

     use Sympa::LockedFile;
     
     # Create filehandle acquiring lock.
     my $fh = Sympa::LockedFile->new('/path/to/file', 20, '+<') or die;
     # or,
     my $fh = Sympa::LockedFile->new();
     $fh->open('/path/to/file', 20, '+<') or die;
     
     # Operations...
     while (<$fh>) { ... }
     seek $fh, 0, 0;
     truncate $fh, 0;
     print $fh "blah blah\n";
     # et cetera.
    
     # Close filehandle releasing lock.
     $fh->close;

# DESCRIPTION

This class implements a filehadle with locking.

## Class Methods

- new ( \[ $file, \[ $blocking\_timeout, \[ $mode \] \] \] )

    Creates new object.
    If any of optional parameters are specified, opens a file acquiring lock.

    Parameters:

    See open().

    Returns:

    New object or, if something went wrong, false value.

- last\_error ( )

    Get a string describing the most recent error.

    Parameters:

    None.

    Returns:

    String or, if recent operation was success, `undef`. 

## Instance Methods

Instances of [Sympa::LockedFile](./Sympa-LockedFile.3.md) support the methods provided by [IO::File](https://metacpan.org/pod/IO::File).

- open ( $file, \[ $blocking\_timeout, \[ $mode \] \] )

    Opens a file specified by $file acquiring lock.

    Parameters:

    - $file

        Path of file to be locked and opened.

    - $blocking\_timeout

        Programs will block up to the number of seconds specified by this option
        before returning undef (could not get a lock).
        If negative value was given, programs will not block but fail immediately
        (`LOCK_NB`).

        Default is `30`.

        However, if existing lock is older than 1200 seconds i.e. 20 minutes,
        lock will be stolen.

    - $mode

        Mode to open file.
        If it implys any writing operations (`'>'`, `'>>'`,
        `'+<'`, ...), trys to acquire exclusive lock (`LOCK_EX`),
        otherwise shared lock (`LOCK_SH`).

        Default is `'<'`.

        Additionally, a special mode `'+'` will acquire exclusive lock
        without opening file.  In this case the file does not have to exist.

        The numeric modes used for sysopen() (e.g. `O_CREAT`) are not supported.

    Returns:

    New filehandle.
    If acquiring lock failed, won't open file.
    If opening file failed, releases acquired lock.
    In both cases returns false value.

- close ( )

    Closes filehandle and releases lock on it.

    Note that destruction of instance will safely close filehandle and release
    lock.

    Parameters:

    None.

    Returns:

    If close succeeded, returns true value, otherwise false value.

    If filehandle had not been locked by current process,
    this method will safely close it and die.

Following methods are specific to this module.

- basename ( \[ $level \] )

    Gets base name of locked file.

- extend ( )

    Extends stale lock timeout.
    The lock will never be stolen in 1200 seconds again.

    Parameters:

    None.

    Returns:

    If extension succeeded, returns true value, otherwise false value.

    If filehandle had not been locked by current process,
    this method will die doing nothing.

- rename ( $destfile )

    Renames file, closes filehandle and releases lock on it.
    Filehandle must have acquired exclusive lock.

    Parameter:

    - $destfile

        Destination path of renaming.

    Returns:

    If renaming succeeded, returns true value, otherwise false value
    and does not release lock.
    In both cases filehandle is closed anyway.

    If filehandle had not been locked by current process,
    this method will die doing nothing.

- unlink ( )

    Deletes file and releases lock on it.

    Parameters:

    None.

    Returns:

    If unlink succeeded, returns true value, otherwise false value and
    does not release lock.

    If filehandle had not been locked by current process,
    this method will die without deleting file.

# SEE ALSO

["Functions for filehandles, files or directories" in perlfunc](https://metacpan.org/pod/perlfunc#Functions-for-filehandles-files-or-directories),
["I/O Operators" in perlop](https://metacpan.org/pod/perlop#I-O-Operators),
[IO::File](https://metacpan.org/pod/IO::File), [File::NFSLock](https://metacpan.org/pod/File::NFSLock).

# HISTORY

Lock module written by Olivier Salaün appeared on Sympa 5.3.

Support for NFS was added by Kazuo Moriwaka.

Rewritten [Sympa::LockedFile](./Sympa-LockedFile.3.md) module was initially written by IKEDA Soji
for Sympa 6.2.
