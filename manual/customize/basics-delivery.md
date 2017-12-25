---
title: 'Message delivery'
prev: basics-addresses.md
up: basics-workflow.md
next: basics-roles.md
---

Message delivery
================

Message reception modes
-----------------------

List members can select a reception mode for messages sent through the list.
Here is a list of the all reception modes:

  - `mail`:

    Standard (direct delivery).

  - `notice`:

    Sends only the subject of messages.

  - `txt`:

    Sends only plain text part of `multipart/alternative` messages.

  - `urlize`:

    Replaces attachments with the links to the file in message archive.

  - `digest`:

    Digest delivery (MIME `multipart/digest` format).

  - `digestplain`:

    Digest delivery (plain text format).

  - `summary`:

    Digest delivery, sort of: Sends the list of links to message archive.

  - `nomail`:

    No delivery.

  - `not_me`:

    No delivery, only if the recipient is originator of the message.

----
Note:

  * `html` reception mode was deprecated on Sympa 6.2.24. Like `txt` mode,
    this mode intended to send *HTML part* of multipart messages, and
    therefore not practically useful.

----

List members can choose the reception mode of their own either through the web
interface or via the `SET `*list*` `*mode* mail command.

The available reception modes can be restricted by listmasters and/or list
owners with the
[`available_user_options`](../man/list_config.5.md#available_user_options)
list/global parameter.  list owners can define default reception mode for
users added to the list with
[`default_user_options`](../man/list_config.5.md#default_user_options)
list parameter.

Message topics
--------------

A list can be configured to have message topics (this notion is different from topics used to class mailing lists). Users can subscribe to these message topics in order to receive a subset of distributed messages: a message can have one or more topics and subscribers will receive only messages that have been tagged with a topic they are subscribed to. A message can be tagged automatically, by the message sender or by the list moderator.

### Message topic definition in a list

Available message topics are defined by list parameters. For each new message topic, create a new [`msg_topic`](../man/list_config.5.md#msg_topic) paragraph that defines the name and the title of the topic. If a thread is identified for the current message, then the automatic procedure is performed. Otherwise, to use automatic tagging, you should define keywords. To define which part of the message is used for automatic tagging, you have to define the [`msg_topic_keywords_apply_on`](../man/list_config.5.md#msg_topic_keywords_apply_on) list parameter. Tagging a message can be optional or required, depending on the [`msg_topic_tagging`](../man/list_config.5.md#msg_topic_tagging) list parameter.

### Subscribing to message topics for list subscribers

This feature is only available with the `mail` (normal) delivery mode. Subscribers can select a message topic to receive messages tagged with this topic. To receive messages that were not tagged, users can subscribe to the topic `other`. The message topics selected by a subscriber are stored in the [`subscriber_table`](../man/sympa_database.5.md#subscriber_table) table of Sympa database.

### Message tagging

First of all, if one or more `msg_topic.keywords` are defined, Sympa tries to tag messages automatically. To trigger manual tagging, by message sender or list moderator, on the web interface, Sympa uses authorization scenarios: if the resulting action is `editorkey` (for example in scenario `send.editorkey`), the list moderator is asked to tag the message. If the resulted action is `request_auth` (for example in scenario `send.privatekey`), the message sender is asked to tag the message. The following variables are available as scenario variables to customize tagging: `topic`, `topic-sender`, `topic-editor`, `topic-auto`, `topic-needed` (see [Authorization scenarios](basics-scenarios.md)). If message tagging is required and if it was not yet performed, Sympa will ask the list moderator.

Tagging a message will create a topic information file in "topic spool", the [``$SPOOLDIR``](../layout.md#spooldir)`/topic/` directory. Its name is based on the listname and the Message ID. For message distribution, a `X-Sympa-Topic` field is added to the message, to allow members to use email filters.

