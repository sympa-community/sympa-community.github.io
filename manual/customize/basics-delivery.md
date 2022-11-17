---
title: 'Message delivery'
prev: basics-alterations.md
up: basics-workflow.md
next: basics-roles.md
---

Message delivery
================

The list members and owners may choose the format and/or topics of messages
delivered through a list by each list member. Moreover, the list member
may suspend all delivery for them as long as necessary.

Suspended subscription
----------------------

List members can suspend subscription of their own using `suboptions` page on
web interface (the list owner can suspend or restore subscription of any
list members).  Thus, the members won't receive any posts through the list.
Suspension will continue for the duration the member specified, or forever
until the member will restore subscription.

Suspending members are marked "suspended" in the list review page on web
interface.

Regular delivery and digest delivery
------------------------------------

> **Note**
>
>   * See also
>     "[Message reception modes](basics-alterations.md#message-reception-modes)"
>     in "Message alteration".

List members can select a reception mode for messages sent through the list.

With one of [regular delivery](basics-alterations.md#regular-delivery) modes,
The message is delivered immediately, if it is allowed.

*However*, if [`delivery_time`](/gpldoc/man/sympa_config.5.html#delivery_time)
list configuration parameter is set, messages will be delivered at that time,
and delivery of messages sent after that time will be postponed to the next
day.

With one of [digest delivery](basics-alterations.md#digest-delivery) modes,
multiple messages are compiled in one message which is delivered periodically.

The list owner can set the days and time to deliver compiled message using
[`digest`](/gpldoc/man/sympa_config.5.html#digest) list parameter.

Message topics
--------------

> **Note**
>
>   * Don't confuse with [list topics](/gpldoc/man/sympa_config.5.html#topics).

This feature is available only with regular delivery described above.

A list can be configured to have message topics. Users can subscribe to these
message topics in order to receive a subset of distributed messages:
A message can have
one or more topics and list members will receive only messages that have been
tagged with a topic they are subscribed to. A message can be tagged
automatically, by the message sender or by the list moderator.

To filter messages using message topics by each user, steps below are followed:

  1. Message topic definition in a list

     The list owner defines available message topics by list parameters. For
     each new message topic, they create a new
     [`msg_topic`](/gpldoc/man/sympa_config.5.html#msg_topic) paragraph that defines
     the name, the title of the topic and optional keywords.

  2. Subscribing to message topics for list members.

     The list members can select a message topic to receive messages tagged
     with particular topics using `suboptions` page
     (To receive messages that were not tagged, users can
     subscribe to the topic `other`).
     The message topics selected by a list member are stored in the
     [`subscriber_table`](/gpldoc/man/sympa_database.5.html#subscriber_table) table
     of Sympa database.

  3. Message tagging

     If one or more [`msg_topic`](/gpldoc/man/sympa_config.5.html#msg_topic) are
     defined, Sympa tries to tag messages:

       - If the list is held for moderation by scenario,
         moderator can tag topics on the held message (to hold the message,
         scenario have to return `editorkey`. See also description on special
         scenario variables below).

       - If a thread is identified for the
         current message, the topics tagged for that thread is tagged
         automatically.

       - Otherwise, if `msg_topic.keyword` defined,
         Sympa tries automatic tagging.  The
         [`msg_topic_keywords_apply_on`](/gpldoc/man/sympa_config.5.html#msg_topic_keywords_apply_on)
         determines which part of the message (subject, body or both) is used.

       - Otherwise, if
         [`msg_topic_tagging`](/gpldoc/man/sympa_config.5.html#msg_topic_tagging)
         defines that topic tagging is required, the message is held
         (whether the list is moderated or not) and
         the list moderator or the sender is asked to tag the message.
         In this case the list moderator can tag topics manually on web
         interface.

  4. List members receive only the messages tagged topics they subscribes.

Special scenario variables are available to customize tagging:
`topic`, `topic-sender`, `topic-editor`, `topic-auto` and `topic-needed`
(see [Authorization scenarios](basics-scenarios.md) and
"[Variables](/gpldoc/man/sympa_scenario.5.html#variables)" in `sympa_scenario(5)`).

Tagging a message will create a topic information file in "topic spool",
the [``$SPOOLDIR``](../layout.md#spooldir)`/topic/` directory. Its name
is based on the list name and the message ID. For message distribution, a
`X-Sympa-Topic` field is added to the message, to allow members to use
email filters.

