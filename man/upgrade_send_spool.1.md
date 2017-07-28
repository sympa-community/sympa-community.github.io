# NAME

upgrade\_send\_spool, upgrade\_send\_spool.pl - Upgrade messages in incoming spool

# SYNOMSIS

    upgrade_send_spool.pl [ --dry_run ]

# DESCRIPTION

On Sympa earlier than 6.2, messages sent from WWSympa were injected in
msg spool with special checksum.
Recent release of Sympa and WWSympa injects outbound messages in outgoing
spool or sends them by Mailer directly.
This program migrates messages with old format in appropriate spools.

# OPTIONS

- --dry\_run

    Shows what will be done but won't really perform upgrade process.

# RETURN VALUE

This program exits with status 0 if processing secceeded.
Otherwise exits with non-zero status.

# CONFIGURATION OPTIONS

Following site configuration parameters in `/etc/sympa/sympa.conf` are referred.

- cookie
- queue
- umask

# SEE ALSO

[sympa.conf(5)](./sympa.conf.5.md), [Sympa::Message](./Sympa-Message.3.md).

# HISTORY

upgrade\_send\_spool.pl appeared on Sympa 6.2.
