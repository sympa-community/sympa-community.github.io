Message reception modes
=======================

List members can select a reception mode for messages sent to the list. Here is a list of the reception modes: mail, notice, digest, summary, nomail, txt, html, urlize, not\_me. The reception mode can be selected either through the web interface or via the `SET <list> <mode>` mail command. The available reception modes can be restricted by listmaster/listowner through the [available_user_options](http://www.sympa.org/manual/parameters-sending#available_user_options) list/global parameter.

Message topics
--------------

A list can be configured to have message topics (this notion is different from topics used to class mailing lists). Users can subscribe to these message topics in order to receive a subset of distributed messages: a message can have one or more topics and subscribers will receive only messages that have been tagged with a topic they are subscribed to. A message can be tagged automatically, by the message sender or by the list moderator.

### Message topic definition in a list

Available message topics are defined by list parameters. For each new message topic, create a new `msg_topic` paragraph that defines the name and the title of the topic. If a thread is identified for the current message, then the automatic procedure is performed. Otherwise, to use automatic tagging, you should define keywords (see [msg_topic](/manual/parameters-sending#msg_topic)). To define which part of the message is used for automatic tagging, you have to define the `msg_topic_keywords_apply_on` list parameter (see [msg_topic_keywords_apply_on](/manual/parameters-sending#msg_topic_keywords_apply_on)). Tagging a message can be optional or required, depending on the [msg_topic_tagging list parameter](/manual/parameters-sending#msg_topic_tagging_list_parameter).

### Subscribing to message topics for list subscribers

This feature is only available with the `normal` delivery mode. Subscribers can select a message topic to receive messages tagged with this topic. To receive messages that were not tagged, users can subscribe to the topic `other`. The message topics selected by a subscriber are stored in the Sympa database (`subscriber_table` table).

### Message tagging

First of all, if one or more `msg_topic.keywords` are defined, Sympa tries to tag messages automatically. To trigger manual tagging, by message sender or list moderator, on the web interface, Sympa uses authorization scenarios: if the resulting action is `editorkey` (for example in scenario `send.editorkey`), the list moderator is asked to tag the message. If the resulted action is `request_auth` (for example in scenario `send.privatekey`), the message sender is asked to tag the message. The following variables are available as scenario variables to customize tagging: `topic`, `topic-sender`, `topic-editor`, `topic-auto`, `topic-needed` (see [Authorization scenarios](/manual/authorization-scenarios#authorization_scenarios)). If message tagging is required and if it was not yet performed, Sympa will ask the list moderator.

Tagging a message will create a topic information file in the `/home/sympa/spool/topic/` spool. Its name is based on the listname and the Message-ID. For message distribution, a `X-Sympa-Topic` field is added to the message, to allow members to use email filters.

Multipart/alternative
---------------------

If available, list members can select the **TXT** or **HTML** reception modes. In these modes, the list member will receive the selected version of a message if the message's content-type is multipart/alternative.
