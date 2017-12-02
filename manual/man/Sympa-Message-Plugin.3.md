# NAME

Sympa::Message::Plugin - process hooks

# SYNOPSIS

    Sympa::Message::Plugin::execute('post_archive', $message);

# DESCRIPTION

Sympa::Message::Plugin provides hook mechanism to intervene in processing by
Sympa.
Each hook may modify objects (messages and so on) or may break ordinary
processing.

**Notice**:
Hook mechanism is expreimental.
Module names and interfaces may be changed in the future.

## Methods

- execute ( HOOK\_NAME, MESSAGE, \[ KEY => VAL, ... \] )

    Process message hook.

## Hooks

Currently, following hooks are supported:

- pre\_distribute

    _Message hook_.
    Message had been approved distribution (by scenario or by moderator), however,
    it has not been decorated (adding custom subject etc.) nor archived yet.

- post\_archive

    _Message hook_.
    Message had been archived, however, it has not been distributed to users
    including digest spool; message has not been signed nor encrypted (if
    necessary).

## How to add a hook to your Sympa

First, write your hook module:

    package My::Hook;

    use constant gettext_id => 'My message hook';
    
    sub post_archive {
        my $module  = shift;    # module name: "My::Hook"
        my $name    = shift;    # handler name: "post_archive"
        my $message = shift;    # Message object
        my %options = @_;
    
        # Processing, possiblly changing $message...
    
        # Return suitable result.
        # If unrecoverable error occurred, you may return undef or simply die.
        return 1;
    }
    
    1;

Then activate hook handler in your list config:

    message_hook
      post_archive My::Hook

# SEE ALSO

[Sympa::Message::Plugin::FixEncoding](./Sympa-Message-Plugin-FixEncoding.3.md) - An example module for message hook.

# HISTORY

[Sympa::Message::Plugin](./Sympa-Message-Plugin.3.md) appeared on Sympa 6.2.
It was initially written by IKEDA Soji <ikeda@conversion.co.jp>.
