# NAME

Sympa::Message::Plugin::FixEncoding -
Example module for message hook to correct charset and encoding of messages

# DESCRIPTION

This hook module corrects charset (character set) and transfer-encoding
of messages distributed from list according to list's language configuration.
It won't affect to archived messages.

For more details about Sympa message hook see [Sympa::Message::Plugin](./Sympa::Message::Plugin.3.md).

This module implements following handler.

- post\_archive

# SEE ALSO

[Sympa::Message::Plugin](./Sympa::Message::Plugin.3.md)

# AUTHOR

IKEDA Soji <ikeda@conversion.co.jp>.
