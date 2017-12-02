# NAME

Sympa::Internals::Workflow - Overview on workflow of Sympa

# DESCRIPTION

Following picture roughly describes interaction among several classes in
workflow of Sympa.  For more details see documentation on each class.

## Message processing

    <<archived.pl>>
    
    Archive => [ProcessArchive] => (list archive)
    
    <<bounced.pl>>
    
    Bounce => [ProcessBounce] => Tracking
    
    <<bulk.pl>>
    
    Outgoing => [ProcessOutgoing] => (Mailer)
    
    <<sympa_automatic.pl>>
    
    Automatic => [ProcessAutomatic] => Incoming
    
    <<sympa_msg.pl>>
    
    Digest::Collection => [ProcessDigest]
                                       :
                                       *1
    
                        +-> (reject or ignore)
                       /
                      +---> [DoCommand]
                     /               :
    Incoming => [ProcessIncoming]    *2
                     \                              +-> (reject)
                      +-> [DoForward] => (Mailer)  /
                       \                          +-> [ToEditor] => Outgoing
                        +-> [DoMessage]          /
                                  \             /---> [ToHeld] => Held
             *3 (CONFIRM)          +-> [AuthorizeMessage]
             :                    /             \---> [ToModeration] => Mod.
    Held => [ProcessHeld] -------+               \
                                                  +-> [DistributeMessage]
               *3 (DISTRIBUTE)                   /             \
                  (REJECT)       +--> (reject)  /               \
                   :            /              /                 \
    Moderation => [ProcessModeration]         /                   \
                                \            /                     \
                                 +----------+                       \
                                                                     \
                          +-------------------------------------------+
                           \
                           [TransformIncoming]
    <<wwsympa.fcgi>>         \
                           [ToArchive] => Archive
    (list archive)             \
     => [ResendArchive] -- [TransformOutgoing] -+
                                 \               \
                           [ToDigest] => Digest   \
                                   \               \
                                    +---------------+-> [ToList] => Outgoing
    
                               +-> [TransformDigestFinal]  
                              /                     \
    <<Template sending>>     /         +----------> [ToOutgoing] => Outgoing 
                            /         / 
    (mail template) => [ProcessTemplate] ---------> [ToAlarm] => Alarm
                           :          \
                           *1          +----------> [ToMailer] => (Mailer)

## Command processing

    <<sympa_msg.pl>>
    
                     *2
                     :
    (message) => [ProcessMessage] -+              +-> (reject)
                                    \            /
         *3 (AUTH)                   \          /---> [ToAuth] => Auth
            (DECL)    +-> (decline)   +-> [AuthorizeRequest]
             :       /               /          \---> [ToAuthOwner] => Auth
    Auth => [ProcessAuth]           /            \
                     \             /              +-> [DispatchRequest]
                      +-----------+                            \
                                 /                      (request handler)
    <<wwsympa.fcgi, SOAP>>      /                                  :
                               /                                   *3
    Request::Collection       / 
           => [ProcessRequest]

## Legend

- `_ClassName_`

    Spool class.  Prefix `Sympa::Spool::` is omitted.

    - `Alarm`
    - `Outgoing`
    - `Tracking`

        [Sympa::Alarm](./Sympa-Alarm.3.md), [Sympa::Bulk](./Sympa-Bulk.3.md) and [Sympa::Tracking](./Sympa-Tracking.3.md) classes
        (they are named such by historical reason).

- `[_ClassName_]`

    Workflow class.  Prefix `Sympa::Spindle::` is omitted.

- `(Mailer)`

    [Sympa::Mailer](./Sympa-Mailer.3.md) class.

- `(list archive)`

    [Sympa::Archive](./Sympa-Archive.3.md) class.

- `(mail template)`

    [Sympa::Message::Template](./Sympa-Message-Template.3.md) class.

- `(message)`

    [Sympa::Request::Message](./Sympa-Request-Message.3.md) class.

- `(request handler)`

    A subclass of `Sympa::Request::Handler` class.

# SEE ALSO

[sympa\_toc(1)](./sympa_toc.1.md), [Sympa::Internals](./Sympa-Internals.3.md), [Sympa::Spindle](./Sympa-Spindle.3.md), [Sympa::Spool](./Sympa-Spool.3.md).
