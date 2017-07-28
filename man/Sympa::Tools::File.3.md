# NAME

Sympa::Tools::File - File-related functions

# DESCRIPTION

This package provides some file-related functions.

## Functions

- set\_file\_rights(%parameters)

    Sets owner and/or access rights on a file.

    Returns true value if setting rights succeeded.
    Otherwise returns false value.

    Note:
    If superuser was specified as owner, this function will die.

- copy\_dir($dir1, $dir2)

    Copy a directory and its content

- del\_dir($dir)

    Delete a directory and its content

- mk\_parent\_dir($file)

    To be used before creating a file in a directory that may not exist already.

- mkdir\_all($path, $mode)

    Recursively create directory and all parent directories

- shift\_file($file, $count)

    Shift file renaming it with date. If count is defined, keep $count file and
    unlink others

- get\_mtime ( $file )

    Gets modification time of the file.

    Parameter:

    - $file

        Full path of file.

    Returns:

    Modification time as UNIX time.
    If the file is not found (including the case that the file vanishes during
    execution of this function) or is not readable, returns `POSIX::INT_MIN`.
    In case of other error, returns `undef`.

- list\_dir($dir, $all, $original\_encoding)

    DEPRECATED.

    Recursively list the content of a directory
    Return an array of hash, each entry with directory + filename + encoding

- get\_dir\_size($dir)

    TBD.

- qencode\_hierarchy()

    DEPRECATED.

    Q-encodes a complete file hierarchy.
    Useful to Q-encode subshared documents.

    ToDo:
    See a comment on ["qencode\_filename" in Sympa::Tools::Text](./Sympa::Tools::Text.3.md#qencode_filename).

- remove\_dir(@directories)

    Function for Removing a non-empty directory.
    It takes a variale number of arguments:
    It can be a list of directory or few direcoty paths.

# HISTORY

[Sympa::Tools::File](./Sympa::Tools::File.3.md) appeared on Sympa 6.2a.41.
