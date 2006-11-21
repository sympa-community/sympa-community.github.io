Reception mode
==============

Message topics
--------------

A list can be configured to have message topics (this notion is different from topics used to class mailing lists). Users can subscribe to these message topics in order to receive a subset of distributed messages : a message can have one or more topics and subscribers will receive only messages that have been tagged with a topic they are subscribed to. A message can be tagged automatically, by the message sender or by the list moderator.

### Message topic definition in a list

Available message topics are defined by list parameters. Foreach new message topic, create a new `msg_topic` paragraph that defines the name and the title of the topic. If a thread is identified for the current message then the automatic procedure is performed. Else, to use automatic tagging, you should define keywords (See ([21.4.13](node22.html#par-msg-topic), page [![\[\*\]](crossref.png)](node22.html#par-msg-topic)) To define which part of the message is used for automatic tagging you have to define `msg_topic_keywords_apply_on` list parameter (See [21.4.14](node22.html#par-msg-topic-key-apply-on), page [![\[\*\]](crossref.png)](node22.html#par-msg-topic-key-apply-on)). Tagging a message can be optional or it can be required, depending on the `msg_topic_tagging` list parameter (See ([21.4.15](node22.html#par-msg-topic-tagging),page [![\[\*\]](crossref.png)](node22.html#par-msg-topic-tagging)).

### Subscribing to message topic for list subscribers

This functionnality is only available via \`\`normal'' reception mode. Subscribers can select message topic to receive messages tagged with this topic. To receive messages that were not tagged, users can subscribe to the topic \`\`other''. Message topics selected by a subscriber are stored in *Sympa* database (subscriber\_table table).

### Message tagging

First of all, if one or more `msg_topic.keywords` are defined, *Sympa* tries to tag messages automatically. To trigger manual tagging, by message sender or list moderator, on the web interface, *Sympa* uses authorization scenarios : if the resulted action is \`\`editorkey'' (for example in scenario send.editorkey), the list moderator is asked to tag the message. If the resulted action is \`\`request\_auth'' (for example in scenario send.privatekey), the message sender is asked to tag the message. The following variables are available as scenario variables to customize tagging : topic, topic-sender, topic-editor, topic-auto, topic-needed. (See ([14][(]customize/basics-scenarios.md#scenarios), page [![\[\*\]](crossref.png)][(]customize/basics-scenarios.md#scenarios)) If message tagging is required and if it was not yet performed, *Sympa* will ask to the list moderator.

Tagging a message will create a topic information file in the `/usr/local/sympa-os/spool/topic/` spool. Its name is based on the listname and the Message-ID. For message distribution, a \`\`X-Sympa-Topic'' field is added to the message to allow members to use mail filters.

