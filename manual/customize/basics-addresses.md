---
title: 'Mail addresses'
prev: ../customize.md
up: basics-workflow.md
next: basics-alterations.md
---

Mail addresses
==============

To provide mail interface, Sympa requires a set of e-mail addresses for each
mail domain and each list.

Addresses by mail domain
------------------------

These addresses are prepared by each mail domain.
Typically, these addresses are implemented as "mail aliases" managed by
system administrators.

  * `sympa@mail.example.org`

    The destination address of mail command interface.  Messages sent to this
    address will be processed its content as
    ~~[mail commands](../mail-commands.md)~~.

    ----
    Notes:

      * If you decided not to provide mail command interface to users, this
        `sympa` address may be disabled.
        However, you have to modify many mail templates to remove links to
        this address from system messages.

      * Though you might use the other mailbox than `sympa` by setting
        [`email`](../man/sympa.conf.5.md#email) parameter, you will have to
        change mail aliases as well.

    ----

  * `listmaster@mail.example.org`

    The public address to contact the [listmasters](basics-roles.md).  Messages
    sent to this address will be forwarded to the people configured by
    [`listmaster`](../man/sympa.conf.5.md#listmaster) parameter.
    
    ----
    Note:
    
      * Though you might use the other mailbox than `listmaster` by setting
        [`listmaster_email`](../man/sympa.conf.5.md#listmaster_email)
        parameter, you will have to change mail aliases as well.
    
    ----

  * `bounce+`*parameters*`@mail.example.org`

    The return path (destination of the report on the result of message
    delivery) of VERP messages.  Messages sent to this address will be analysed
    to get information including original recipients.

    Original receipents of the messages sent through the lists are encoded in
    *parameters* so that Sympa can detect which users are bouncing.  See
    "[Bounce management: VERP](bounce-management.md#verp)" for more details.

    This address also is used for [message tracking](message-tracking.md). In
    such case *parameters* will contain envelope ID along with information of
    original recipient.
    
    ----
    Note:
    
      * Though you might use the other mailboxes than `bounce` and so on by
        setting
        [`bounce_email_prefix`](../man/sympa.conf.5.md#bounce_email_prefix)
        parameter, you will have to change mail aliases as well.
    
    ----

  * `abuse-feedback-report@mail.example.org`

    This address is optional. The address used to receive feedback report of
    ARF.  See "[Bounce management: ARF](bounce-management.md#arf)" for details.

Among addresses above, only listmaster address may forward messages to the
human: Others swallow messages and none will read content of them,

Additionally, following address have to be managed:

  * `sympa-request@mail.example.org`

    The destination of delivery error reports on the messages which are
    originated by or forwarded by mailing list service _and_ are not related
    to particular list.

    Messages bound for this address _must not_ be forwarded to any addresses
    described in this chapter:
    Address of postmaster is recommended destination.

Addresses by list
-----------------

These addresses are prepared by each mailing list.
Typically, these addresses are implemented as "mail aliases" managed by
Sympa's alias management function.

  * *list name*`@mail.example.org`

    The posting address, also sometimes called "list address".  Messages
    sent to this address will be distributed to all list members (unless
    inhibited by list configuration).

  * *list name*`-request@mail.example.org`

    The address to contact the [list owners](basics-roles.md).  Messages sent
    to this address will be forwarded to the people designated as list owners
    according to list configuration.

  * *list name*`-editor@mail.example.org`

    The address to contact the [moderators](basics-roles.md).  Messages sent
    to this address will be forwarded to the people designated as moderators
    (editors) according to list configuration.  If no moderators are not
    designated, it will be forwarded to the list owners.

  * *list name*`-owner@mail.example.org`

    This address is _not_ the contact to list owners: This is the destination
    of delivery error reports (bounces).  Messages sent to this address will
    be analysed to get information about original recipients (bouncing users).

    For more details see "[Bounce management](bounce-management.md)".

  * *list name*`-subscribe@mail.example.org`

  * *list name*`-unsubscribe@mail.example.org`

    These addresses are optional.  If these addresses are available, people who
    want to join (or leave) list may do it just sending the message to these
    addresses.

Among addresses above, only list, owner and moderator addresses may forward
messages to the human: Others swallow messages and none will read content of
them.

### Addresses for automatic lists

If automatic list feature is enabled, arbitrary addresses will accept incoming
messages and the list corresponding to that address will be created.  See
"[Automatic list creation](automatic-lists.md)" for more details.
