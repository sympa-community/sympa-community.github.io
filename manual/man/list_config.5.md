---
title: 'list_config(5)'
---

# NAME

list\_config - Configuration file for mailing list

# DESCRIPTION

`config` is main configuration file of the mailing list.

Format of `config` is as following:

- Lines beginning with `#` and containing only spaces are ignored.
- There are parameters with single line and multiple lines:
    - On single line parameters,
    each line has the form "_parameter_ _value_".
    _value_ may contain spaces but may not contain newlines.

        Several parameters may have multiple vlues.
        If it's the case, values may be separated by comma (`,`)
        or parameter lines may be repeated.
        Some of parameters must have one or more of limited values.

        Example:

            subject This is subject of my list
            
            remove_headers User-Agent,Importance
            
            custom_headers X-List: mylist
            custom_headers X-Face: %`-W7!?^]Sg'I-K>P<cdn&k:~A^{x>(]Gc{V...
            
            rfc2369_header_fields post,owner 

    - A multiple line parameter, so-called "paragraph", consists of
    the first line specifying parameter name and subsequent one or more
    sub-parameter lines.  Paragraph must be separated by one or more empty lines
    from the other parameters.

        Several multiple line parameters may occur multiple times.

# PARAMETERS

Below is entire list of configuration parameters.
"Default" is built-in default value if any.

## List definition

### `subject`

Subject of the list

- Format:

    /`.+`/

- Default:

    None, _mandatory_.

This parameter indicates the subject of the list, which is sent in response to the LISTS mail command. The subject is a free form text limited to one line.

### `visibility`

Visibility of the list

- Format:

    Name of `visibility` scenario.

- Default:

    `default`

This parameter indicates whether the list should feature in the output generated in response to a LISTS command or should be shown in the list overview of the web-interface.

### `owner`

Owner

- Multiple occurrences allowed, _mandatory_

Owners are managing subscribers of the list. They may review subscribers and add or delete email addresses from the mailing list. If you are a privileged owner of the list, you can choose other owners for the mailing list. Privileged owners may edit a few more options than other owners. 

#### `owner.email`

email address

- Format:

    /`$email`/

- Default:

    None, _mandatory_.

#### `owner.gecos`

name

- Format:

    /`.+`/

- Default:

    None.

#### `owner.info`

private information

- Format:

    /`.+`/

- Default:

    None.

#### `owner.profile`

profile

- Format:
    - `privileged` - privileged owner
    - `normal` - normal owner
- Default:

    `normal`

#### `owner.reception`

reception mode

- Format:
    - `mail` - receive notification email
    - `nomail` - no notifications
- Default:

    `mail`

#### `owner.visibility`

visibility

- Format:
    - `conceal` - concealed from list menu
    - `noconceal` - listed on the list menu
- Default:

    `noconceal`

### `owner_include`

Owners defined in an external data source

- Multiple occurrences allowed

#### `owner_include.source`

the datasource

- Format:

    /`[\w-]+`/

- Default:

    None, _mandatory_.

#### `owner_include.source_parameters`

datasource parameters

- Format:

    /`.*`/

- Default:

    None.

#### `owner_include.profile`

profile

- Format:
    - `privileged` - privileged owner
    - `normal` - normal owner
- Default:

    `normal`

#### `owner_include.reception`

reception mode

- Format:
    - `mail` - receive notification email
    - `nomail` - no notifications
- Default:

    `mail`

#### `owner_include.visibility`

visibility

- Format:
    - `conceal` - concealed from list menu
    - `noconceal` - listed on the list menu
- Default:

    `noconceal`

### `editor`

Moderators

- Multiple occurrences allowed

Editors are responsible for moderating messages. If the mailing list is moderated, messages posted to the list will first be passed to the editors, who will decide whether to distribute or reject it.

FYI: Defining editors will not make the list moderated; you will have to set the "send" parameter.

FYI: If the list is moderated, any editor can distribute or reject a message without the knowledge or consent of the other editors. Messages that have not been distributed or rejected will remain in the moderation spool until they are acted on.

#### `editor.email`

email address

- Format:

    /`$email`/

- Default:

    None, _mandatory_.

#### `editor.gecos`

name

- Format:

    /`.+`/

- Default:

    None.

#### `editor.info`

private information

- Format:

    /`.+`/

- Default:

    None.

#### `editor.reception`

reception mode

- Format:
    - `mail` - receive notification email
    - `nomail` - no notifications
- Default:

    `mail`

#### `editor.visibility`

visibility

- Format:
    - `conceal` - concealed from list menu
    - `noconceal` - listed on the list menu
- Default:

    `noconceal`

### `editor_include`

Moderators defined in an external data source

- Multiple occurrences allowed

#### `editor_include.source`

the data source

- Format:

    /`[\w-]+`/

- Default:

    None, _mandatory_.

#### `editor_include.source_parameters`

data source parameters

- Format:

    /`.*`/

- Default:

    None.

#### `editor_include.reception`

reception mode

- Format:
    - `mail` - receive notification email
    - `nomail` - no notifications
- Default:

    `mail`

#### `editor_include.visibility`

visibility

- Format:
    - `conceal` - concealed from list menu
    - `noconceal` - listed on the list menu
- Default:

    `noconceal`

### `topics`

Topics for the list

- Format:

    Multiple values allowed, separated by `,`.

    List topic.

- Default:

    None.

This parameter allows the classification of lists. You may define multiple topics as well as hierarchical ones. WWSympa's list of public lists uses this parameter.

### `host`

Internet domain

- Format:

    /`$host`/

- Default:

    Value of [`host`](./sympa.conf.5.md#host) parameter in `sympa.conf` or `robot.conf`.

Domain name of the list, default is the robot domain name set in the related robot.conf file or in file sympa.conf.

### `lang`

Language of the list

- Format:

    Language tag.

- Default:

    Value of [`lang`](./sympa.conf.5.md#lang) parameter in `sympa.conf` or `robot.conf`.

This parameter defines the language used for the list. It is used to initialize a user's language preference; Sympa command reports are extracted from the associated message catalog.

### `family_name`

Family name

- Format:

    /`$family_name`/

- Default:

    None.

### `max_list_members`

Maximum number of list members

- Format:

    Number of list members.

- Default:

    Value of [`default_max_list_members`](./sympa.conf.5.md#default_max_list_members) parameter in `sympa.conf` or `robot.conf`.

limit for the number of subscribers. 0 means no limit.

### `priority`

Priority

- Format:
    - `0` - 0 - highest priority
    - `1` - 1
    - `2` - 2
    - `3` - 3
    - `4` - 4
    - `5` - 5
    - `6` - 6
    - `7` - 7
    - `8` - 8
    - `9` - 9 - lowest priority
    - `z` - queue messages only
- Default:

    Value of [`default_list_priority`](./sympa.conf.5.md#default_list_priority) parameter in `sympa.conf` or `robot.conf`.

The priority with which Sympa will process messages for this list. This level of priority is applied while the message is going through the spool. The z priority will freeze the message in the spool.

## Sending/receiving setup

### `send`

Who can send messages

- Format:

    Name of `send` scenario.

- Default:

    `default`

This parameter specifies who can send messages to the list.

### `delivery_time`

Delivery time (hh:mm)

- Format:

    /`[0-2]?\d\:[0-6]\d`/

- Default:

    None.

If this parameter is present, non-digest messages will be delivered to subscribers at this time: When this time has been past, delivery is postponed to the same time in next day.

### `digest`

Digest frequency

- Single occurrence

Definition of digest mode. If this parameter is present, subscribers can select the option of receiving messages in multipart/digest MIME format, or as a plain text digest. Messages are then grouped together, and compiled messages are sent to subscribers according to the frequency selected with this parameter.

#### `digest.days`

days

- Format:

    Multiple occurrences allowed.

    Day of week, `0` - `6`.

- Default:

    None, _mandatory_.

#### `digest.hour`

hour

- Format:

    /`\d+`/

- Default:

    None, _mandatory_.

#### `digest.minute`

minute

- Format:

    /`\d+`/

- Default:

    None, _mandatory_.

### `digest_max_size`

Digest maximum number of messages

- Format:

    Number of messages.

- Default:

    `25`

### `available_user_options`

Available subscription options

- Single occurrence

#### `available_user_options.reception`

reception mode

- Format:

    Multiple values allowed, separated by `,`.

    Reception mode of list member.

- Default:

    `mail,notice,digest,digestplain,summary,nomail,txt,urlize,not_me`

Only these modes will be allowed for the subscribers of this list. If a subscriber has a reception mode not in the list, Sympa uses the mode specified in the default\_user\_options paragraph.

### `default_user_options`

Subscription profile

- Single occurrence

Default profile for the subscribers of the list.

#### `default_user_options.reception`

reception mode

- Format:

    Reception mode of list member.

- Default:

    `mail`

Mail reception mode.

#### `default_user_options.visibility`

visibility

- Format:

    Visibility mode of list memeber.

- Default:

    `noconceal`

Visibility of the subscriber.

### `msg_topic`

Topics for message categorization

- Multiple occurrences allowed

This paragraph defines a topic used to tag a message of a list, named by msg\_topic.name ("other" is a reserved word), its title is msg\_topic.title. The msg\_topic.keywords entry is optional and allows automatic tagging. This should be a list of keywords, separated by ','.

#### `msg_topic.name`

Message topic name

- Format:

    /`[\-\w]+`/

- Default:

    None, _mandatory_.

#### `msg_topic.keywords`

Message topic keywords

- Format:

    /`[^,\n]+(,[^,\n]+)*`/

- Default:

    None.

#### `msg_topic.title`

Message topic title

- Format:

    /`.+`/

- Default:

    None, _mandatory_.

### `msg_topic_keywords_apply_on`

Defines to which part of messages topic keywords are applied

- Format:
    - `subject` - subject field
    - `body` - message body
    - `subject_and_body` - subject and body
- Default:

    `subject`

This parameter indicates which part of the message is used to perform automatic tagging.

### `msg_topic_tagging`

Message tagging

- Format:
    - `required_sender` - required to post message
    - `required_moderator` - required to distribute message
    - `optional` - optional
- Default:

    `optional`

This parameter indicates if the tagging is optional or required for a list.

### `reply_to_header`

Reply address

- Single occurrence

This defines what Sympa will place in the Reply-To: SMTP header field of the messages it distributes.

#### `reply_to_header.value`

value

- Format:
    - `sender` - sender
    - `list` - list
    - `all` - all
    - `other_email` - other email address
- Default:

    `sender`

This parameter indicates whether the Reply-To: field should indicate the sender of the message (sender), the list itself (list), both list and sender (all) or an arbitrary e-mail address (defined by the other\_email parameter).

Note: it is inadvisable to change this parameter, and particularly inadvisable to set it to list. Experience has shown it to be almost inevitable that users, mistakenly believing that they are replying only to the sender, will send private messages to a list. This can lead, at the very least, to embarrassment, and sometimes to more serious consequences.

#### `reply_to_header.other_email`

other email address

- Format:

    /`$email`/

- Default:

    None.

If value was set to other\_email, this parameter defines the e-mail address used.

#### `reply_to_header.apply`

respect of existing header field

- Format:
    - `forced` - overwrite Reply-To: header field
    - `respect` - preserve existing header field
- Default:

    `respect`

The default is to respect (preserve) the existing Reply-To: SMTP header field in incoming messages. If set to forced, Reply-To: SMTP header field will be overwritten.

### `anonymous_sender`

Anonymous sender

- Format:

    /`.+`/

- Default:

    None.

To hide the sender's email address before distributing the message. It is replaced by the provided email address.

### `custom_header`

Custom header field

- Format:

    Multiple occurrences allowed.

    /`\S+:\s+.*`/

- Default:

    None.

This parameter is optional. The headers specified will be added to the headers of messages distributed via the list. As of release 1.2.2 of Sympa, it is possible to put several custom header lines in the configuration file at the same time.

### `custom_subject`

Subject tagging

- Format:

    /`.+`/

- Default:

    None.

This parameter is optional. It specifies a string which is added to the subject of distributed messages (intended to help users who do not use automatic tools to sort incoming messages). This string will be surrounded by \[\] characters.

### `footer_type`

Attachment type

- Format:
    - `mime` - add a new MIME part
    - `append` - append to message body
- Default:

    `mime`

List owners may decide to add message headers or footers to messages sent via the list. This parameter defines the way a footer/header is added to a message.

mime: 

The default value. Sympa will add the footer/header as a new MIME part.

append: 

Sympa will not create new MIME parts, but will try to append the header/footer to the body of the message. Predefined message-footers will be ignored. Headers/footers may be appended to text/plain messages only.

### `max_size`

Maximum message size

- Format:

    Number of bytes.

- Default:

    Value of [`max_size`](./sympa.conf.5.md#max_size) parameter in `sympa.conf` or `robot.conf`.

Maximum size of a message in 8-bit bytes.

### `merge_feature`

Allow message personalization

- Format:
    - `on` - enabled
    - `off` - disabled
- Default:

    Value of [`merge_feature`](./sympa.conf.5.md#merge_feature) parameter in `sympa.conf`.

### `message_hook`

Hook modules for message processing

- Single occurrence

#### `message_hook.pre_distribute`

A hook on the messages before distribution

- Format:

    /`(::|\w)+`/

- Default:

    None.

#### `message_hook.post_archive`

A hook on the messages just after archiving

- Format:

    /`(::|\w)+`/

- Default:

    None.

### `reject_mail_from_automates_feature`

Reject mail from automatic processes (crontab, etc)?

- Format:
    - `on` - enabled
    - `off` - disabled
- Default:

    Value of [`reject_mail_from_automates_feature`](./sympa.conf.5.md#reject_mail_from_automates_feature) parameter in `sympa.conf`.

### `remove_headers`

Incoming SMTP header fields to be removed

- Format:

    Multiple values allowed, separated by `,`.

    /`\S+`/

- Default:

    Value of [`remove_headers`](./sympa.conf.5.md#remove_headers) parameter in `sympa.conf`.

### `remove_outgoing_headers`

Outgoing SMTP header fields to be removed

- Format:

    Multiple values allowed, separated by `,`.

    /`\S+`/

- Default:

    Value of [`remove_outgoing_headers`](./sympa.conf.5.md#remove_outgoing_headers) parameter in `sympa.conf`.

### `rfc2369_header_fields`

RFC 2369 Header fields

- Format:

    Multiple values allowed, separated by `,`.

    - `help` - help
    - `subscribe` - subscription
    - `unsubscribe` - unsubscription
    - `post` - posting address
    - `owner` - owner
    - `archive` - list archive

- Default:

    Value of [`rfc2369_header_fields`](./sympa.conf.5.md#rfc2369_header_fields) parameter in `sympa.conf`.

### `forced_reply_to`

Deprecated.

### `reply_to`

Deprecated.

## Privileges

### `info`

Who can view list information

- Format:

    Name of `info` scenario.

- Default:

    `default`

### `subscribe`

Who can subscribe to the list

- Format:

    Name of `subscribe` scenario.

- Default:

    `default`

The subscribe parameter defines the rules for subscribing to the list.

### `add`

Who can add subscribers

- Format:

    Name of `add` scenario.

- Default:

    `default`

Privilege for adding (ADD command) a subscriber to the list

### `unsubscribe`

Who can unsubscribe

- Format:

    Name of `unsubscribe` scenario.

- Default:

    `default`

This parameter specifies the unsubscription method for the list. Use open\_notify or auth\_notify to allow owner notification of each unsubscribe command.

### `del`

Who can delete subscribers

- Format:

    Name of `del` scenario.

- Default:

    `default`

### `invite`

Who can invite people

- Format:

    Name of `invite` scenario.

- Default:

    `default`

### `remind`

Who can start a remind process

- Format:

    Name of `remind` scenario.

- Default:

    `default`

This parameter specifies who is authorized to use the remind command.

### `review`

Who can review subscribers

- Format:

    Name of `review` scenario.

- Default:

    `default`

This parameter specifies who can access the list of members. Since subscriber addresses can be abused by spammers, it is strongly recommended that you only authorize owners or subscribers to access the subscriber list. 

### `owner_domain`

Required domains for list owners

- Format:

    /`$host( +$host)*`/

- Default:

    Value of [`owner_domain`](./sympa.conf.5.md#owner_domain) parameter in `sympa.conf` or `robot.conf`.

Restrict list ownership to addresses in the specified domains.

### `owner_domain_min`

Minimum owners in required domains

- Format:

    /`\d+`/

- Default:

    Value of [`owner_domain_min`](./sympa.conf.5.md#owner_domain_min) parameter in `sympa.conf` or `robot.conf`.

Require list ownership by a minimum number of addresses in the specified domains.

### `shared_doc`

Shared documents

- Single occurrence

This paragraph defines read and edit access to the shared document repository.

#### `shared_doc.d_read`

Who can view

- Format:

    Name of `d_read` scenario.

- Default:

    `default`

#### `shared_doc.d_edit`

Who can edit

- Format:

    Name of `d_edit` scenario.

- Default:

    `default`

#### `shared_doc.quota`

quota

- Format:

    Number of Kbytes.

- Default:

    Value of [`default_shared_quota`](./sympa.conf.5.md#default_shared_quota) parameter in `sympa.conf` or `robot.conf`.

## Archives

### `process_archive`

Store distributed messages into archive

- Format:
    - `on` - enabled
    - `off` - disabled
- Default:

    Value of [`process_archive`](./sympa.conf.5.md#process_archive) parameter in `sympa.conf` or `robot.conf`.

### `archive`

Archives

- Single occurrence

Privilege for reading mail archives and frequency of archiving

Defines who can access the web archive for the list.

#### `archive.period`

Deprecated.

#### `archive.access`

Deprecated.

#### `archive.web_access`

access right

- Format:

    Name of `access_web_archive` scenario.

- Default:

    None.

#### `archive.mail_access`

access right by mail commands

- Format:

    Name of `archive_mail_access` scenario.

- Default:

    `default`

#### `archive.quota`

quota

- Format:

    Number of Kbytes.

- Default:

    Value of [`default_archive_quota`](./sympa.conf.5.md#default_archive_quota) parameter in `sympa.conf`.

#### `archive.max_month`

Maximum number of month archived

- Format:

    Number of months.

- Default:

    None.

### `archive_crypted_msg`

Archive encrypted mails as cleartext

- Format:
    - `original` - original messages
    - `decrypted` - decrypted messages
- Default:

    `original`

### `web_archive_spam_protection`

email address protection method

- Format:
    - `cookie` - use HTTP cookie
    - `javascript` - use JavaScript
    - `at` - replace @ characters
    - `none` - do nothing
- Default:

    Value of [`web_archive_spam_protection`](./sympa.conf.5.md#web_archive_spam_protection) parameter in `sympa.conf` or `robot.conf`.

Idem spam\_protection is provided but it can be used only for web archives. Access requires a cookie, and users must submit a small form in order to receive a cookie before browsing the archives. This blocks all robot, even google and co.

### `web_archive`

Deprecated.

## Bounces

### `bounce`

Bounces management

- Single occurrence

#### `bounce.warn_rate`

warn rate

- Format:

    Number of %.

- Default:

    Value of [`bounce_warn_rate`](./sympa.conf.5.md#bounce_warn_rate) parameter in `sympa.conf`.

The list owner receives a warning whenever a message is distributed and the number (percentage) of bounces exceeds this value.

#### `bounce.halt_rate`

Deprecated.

### `bouncers_level1`

Management of bouncers, 1st level

- Single occurrence

Level 1 is the lower level of bouncing users

#### `bouncers_level1.rate`

threshold

- Format:

    Number of points.

- Default:

    Value of [`default_bounce_level1_rate`](./sympa.conf.5.md#default_bounce_level1_rate) parameter in `sympa.conf` or `robot.conf`.

Each bouncing user have a score (from 0 to 100).

This parameter defines a lower limit for each category of bouncing users.For example, level 1 begins from 45 to level\_2\_treshold.

#### `bouncers_level1.action`

action for this population

- Format:
    - `remove_bouncers` - remove bouncing users
    - `notify_bouncers` - send notify to bouncing users
    - `none` - do nothing
- Default:

    `notify_bouncers`

This parameter defines which task is automaticaly applied on level 1 bouncers.

#### `bouncers_level1.notification`

notification

- Format:
    - `none` - do nothing
    - `owner` - owner
    - `listmaster` - listmaster
- Default:

    `owner`

When automatic task is executed on level 1 bouncers, a notification email can be send to listowner or listmaster.

### `bouncers_level2`

Management of bouncers, 2nd level

- Single occurrence

Level 2 is the highest level of bouncing users

#### `bouncers_level2.rate`

threshold

- Format:

    Number of points.

- Default:

    Value of [`default_bounce_level2_rate`](./sympa.conf.5.md#default_bounce_level2_rate) parameter in `sympa.conf` or `robot.conf`.

Each bouncing user have a score (from 0 to 100).

This parameter defines the score range defining each category of bouncing users.For example, level 2 is for users with a score between 80 and 100.

#### `bouncers_level2.action`

action for this population

- Format:
    - `remove_bouncers` - remove bouncing users
    - `notify_bouncers` - send notify to bouncing users
    - `none` - do nothing
- Default:

    `remove_bouncers`

This parameter defines which task is automaticaly applied on level 2 bouncers.

#### `bouncers_level2.notification`

notification

- Format:
    - `none` - do nothing
    - `owner` - owner
    - `listmaster` - listmaster
- Default:

    `owner`

When automatic task is executed on level 2 bouncers, a notification email can be send to listowner or listmaster.

### `verp_rate`

percentage of list members in VERP mode

- Format:
    - `100%` - 100% - always
    - `50%` - 50%
    - `33%` - 33%
    - `25%` - 25%
    - `20%` - 20%
    - `10%` - 10%
    - `5%` - 5%
    - `2%` - 2%
    - `0%` - 0% - never
- Default:

    Value of [`verp_rate`](./sympa.conf.5.md#verp_rate) parameter in `sympa.conf` or `robot.conf`.

### `tracking`

Message tracking feature

- Single occurrence

#### `tracking.delivery_status_notification`

tracking message by delivery status notification

- Format:
    - `on` - enabled
    - `off` - disabled
- Default:

    Value of [`tracking_delivery_status_notification`](./sympa.conf.5.md#tracking_delivery_status_notification) parameter in `sympa.conf`.

#### `tracking.message_disposition_notification`

tracking message by message disposition notification

- Format:
    - `on` - enabled
    - `on_demand` - on demand
    - `off` - disabled
- Default:

    Value of [`tracking_message_disposition_notification`](./sympa.conf.5.md#tracking_message_disposition_notification) parameter in `sympa.conf`.

#### `tracking.tracking`

who can view message tracking

- Format:

    Name of `tracking` scenario.

- Default:

    `default`

#### `tracking.retention_period`

Tracking datas are removed after this number of days

- Format:

    Number of days.

- Default:

    Value of [`tracking_default_retention_period`](./sympa.conf.5.md#tracking_default_retention_period) parameter in `sympa.conf`.

### `welcome_return_path`

Welcome return-path

- Format:
    - `unique` - bounce management
    - `owner` - owner
- Default:

    Value of [`welcome_return_path`](./sympa.conf.5.md#welcome_return_path) parameter in `sympa.conf`.

If set to unique, the welcome message is sent using a unique return path in order to remove the subscriber immediately in the case of a bounce.

### `remind_return_path`

Return-path of the REMIND command

- Format:
    - `unique` - bounce management
    - `owner` - owner
- Default:

    Value of [`remind_return_path`](./sympa.conf.5.md#remind_return_path) parameter in `sympa.conf`.

Same as welcome\_return\_path, but applied to remind messages.

## Data sources setup

### `inclusion_notification_feature`

Notify subscribers when they are included from a data source?

- Format:
    - `on` - enabled
    - `off` - disabled
- Default:

    `off`

### `member_include`

Users included from parameterizable data sources

- Multiple occurrences allowed

#### `member_include.source`

the data source

- Format:

    /`[\w-]+`/

- Default:

    None, _mandatory_.

#### `member_include.source_parameters`

data source parameters

- Format:

    /`.*`/

- Default:

    None.

### `sql_fetch_timeout`

Timeout for fetch of include\_sql\_query

- Format:

    Number of seconds.

- Default:

    Value of [`default_sql_fetch_timeout`](./sympa.conf.5.md#default_sql_fetch_timeout) parameter in `sympa.conf`.

### `include_file`

File inclusion

- Format:

    Multiple occurrences allowed.

    /`\S+`/

- Default:

    None.

Include subscribers from this file.  The file should contain one e-mail address per line (lines beginning with a "#" are ignored).

### `include_remote_file`

Remote file inclusion

- Multiple occurrences allowed

#### `include_remote_file.name`

short name for this source

- Format:

    /`.+`/

- Default:

    None.

#### `include_remote_file.url`

data location URL

- Format:

    /`.+`/

- Default:

    None, _mandatory_.

#### `include_remote_file.user`

remote user

- Format:

    /`.+`/

- Default:

    None.

#### `include_remote_file.passwd`

remote password

- Format:

    The value to be concealed.

- Default:

    None.

### `include_sympa_list`

List inclusion

- Multiple occurrences allowed

Include subscribers from other list. All subscribers of list listname become subscribers of the current list. You may include as many lists as required, using one include\_sympa\_list paragraph for each included list. Any list at all may be included; you may therefore include lists which are also defined by the inclusion of other lists. Be careful, however, not to include list A in list B and then list B in list A, since this will give rise to an infinite loop.

#### `include_sympa_list.name`

short name for this source

- Format:

    /`.+`/

- Default:

    None.

#### `include_sympa_list.listname`

list name to include

- Format:

    /`$listname(\@$host)?`/

- Default:

    None, _mandatory_.

#### `include_sympa_list.filter`

filter definition

- Format:

    /`.*`/

- Default:

    None.

### `include_remote_sympa_list`

remote list inclusion

- Multiple occurrences allowed

Sympa can contact another Sympa service using HTTPS to fetch a remote list in order to include each member of a remote list as subscriber. You may include as many lists as required, using one include\_remote\_sympa\_list paragraph for each included list. Be careful, however, not to give rise to an infinite loop resulting from cross includes.

For this operation, one Sympa site acts as a server while the other one acs as client. On the server side, the only setting needed is to give permission to the remote Sympa to review the list. This is controlled by the review scenario.

#### `include_remote_sympa_list.name`

short name for this source

- Format:

    /`.+`/

- Default:

    None.

#### `include_remote_sympa_list.host`

remote host

- Format:

    /`$host`/

- Default:

    None, _mandatory_.

#### `include_remote_sympa_list.port`

remote port

- Format:

    /`\d+`/

- Default:

    `443`

#### `include_remote_sympa_list.path`

remote path of sympa list dump

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `include_remote_sympa_list.cert`

certificate for authentication by remote Sympa

- Format:
    - `robot` - robot
    - `list` - list
- Default:

    `list`

### `include_ldap_query`

LDAP query inclusion

- Multiple occurrences allowed

This paragraph defines parameters for a query returning a list of subscribers. This feature requires the Net::LDAP (perlldap) PERL module.

#### `include_ldap_query.name`

short name for this source

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_query.host`

remote host

- Format:

    /`$multiple_host_or_url`/

- Default:

    None, _mandatory_.

#### `include_ldap_query.port`

Deprecated.

#### `include_ldap_query.use_tls`

use TLS (formerly SSL)

- Format:
    - `starttls` - use STARTTLS
    - `ldaps` - use LDAPS (LDAP over TLS)
    - `none` - do nothing
- Default:

    `none`

#### `include_ldap_query.ssl_version`

SSL version

- Format:
    - `sslv2` - SSL version 2
    - `sslv3` - SSL version 3
    - `tlsv1` - TLS version 1
    - `tlsv1_1` - TLS version 1.1
    - `tlsv1_2` - TLS version 1.2
- Default:

    `tlsv1`

#### `include_ldap_query.ssl_ciphers`

SSL ciphers used

- Format:

    /`.+`/

- Default:

    `ALL`

#### `include_ldap_query.ca_verify`

Certificate verification

- Format:
    - `none` - do nothing
    - `optional` - optional
    - `required` - required
- Default:

    `required`

#### `include_ldap_query.user`

remote user

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_query.passwd`

remote password

- Format:

    The value to be concealed.

- Default:

    None.

#### `include_ldap_query.suffix`

suffix

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_query.scope`

search scope

- Format:
    - `base` - base
    - `one` - one level
    - `sub` - subtree
- Default:

    `sub`

#### `include_ldap_query.timeout`

connection timeout

- Format:

    Number of seconds.

- Default:

    `30`

#### `include_ldap_query.filter`

filter

- Format:

    /`.+`/

- Default:

    None, _mandatory_.

#### `include_ldap_query.attrs`

extracted attribute

- Format:

    /`$ldap_attrdesc(\s*,\s*$ldap_attrdesc)?`/

- Default:

    `mail`

#### `include_ldap_query.select`

selection (if multiple)

- Format:
    - `all` - all
    - `first` - first entry
- Default:

    `first`

#### `include_ldap_query.nosync_time_ranges`

Time ranges when inclusion is not allowed

- Format:

    /`$time_ranges`/

- Default:

    None.

#### `include_ldap_query.use_ssl`

Obsoleted. Use [`use_tls`](#include_ldap_queryuse_tls).

### `include_ldap_2level_query`

LDAP 2-level query inclusion

- Multiple occurrences allowed

This paragraph defines parameters for a two-level query returning a list of subscribers. Usually the first-level query returns a list of DNs and the second-level queries convert the DNs into e-mail addresses. This feature requires the Net::LDAP (perlldap) PERL module.

#### `include_ldap_2level_query.name`

short name for this source

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_2level_query.host`

remote host

- Format:

    /`$multiple_host_or_url`/

- Default:

    None, _mandatory_.

#### `include_ldap_2level_query.port`

Deprecated.

#### `include_ldap_2level_query.use_tls`

use TLS (formerly SSL)

- Format:
    - `starttls` - use STARTTLS
    - `ldaps` - use LDAPS (LDAP over TLS)
    - `none` - do nothing
- Default:

    `none`

#### `include_ldap_2level_query.ssl_version`

SSL version

- Format:
    - `sslv2` - SSL version 2
    - `sslv3` - SSL version 3
    - `tlsv1` - TLS version 1
    - `tlsv1_1` - TLS version 1.1
    - `tlsv1_2` - TLS version 1.2
- Default:

    `tlsv1`

#### `include_ldap_2level_query.ssl_ciphers`

SSL ciphers used

- Format:

    /`.+`/

- Default:

    `ALL`

#### `include_ldap_2level_query.ca_verify`

Certificate verification

- Format:
    - `none` - do nothing
    - `optional` - optional
    - `required` - required
- Default:

    `required`

#### `include_ldap_2level_query.user`

remote user

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_2level_query.passwd`

remote password

- Format:

    The value to be concealed.

- Default:

    None.

#### `include_ldap_2level_query.suffix1`

first-level suffix

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_2level_query.scope1`

first-level search scope

- Format:
    - `base` - base
    - `one` - one level
    - `sub` - subtree
- Default:

    `sub`

#### `include_ldap_2level_query.timeout1`

first-level connection timeout

- Format:

    Number of seconds.

- Default:

    `30`

#### `include_ldap_2level_query.filter1`

first-level filter

- Format:

    /`.+`/

- Default:

    None, _mandatory_.

#### `include_ldap_2level_query.attrs1`

first-level extracted attribute

- Format:

    /`$ldap_attrdesc`/

- Default:

    None.

#### `include_ldap_2level_query.select1`

first-level selection

- Format:
    - `all` - all
    - `first` - first entry
    - `regex` - entries matching regular expression
- Default:

    `first`

#### `include_ldap_2level_query.regex1`

first-level regular expression

- Format:

    /`.+`/

- Default:

    ``

#### `include_ldap_2level_query.suffix2`

second-level suffix template

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_2level_query.scope2`

second-level search scope

- Format:
    - `base` - base
    - `one` - one level
    - `sub` - subtree
- Default:

    `sub`

#### `include_ldap_2level_query.timeout2`

second-level connection timeout

- Format:

    Number of seconds.

- Default:

    `30`

#### `include_ldap_2level_query.filter2`

second-level filter template

- Format:

    /`.+`/

- Default:

    None, _mandatory_.

#### `include_ldap_2level_query.attrs2`

second-level extracted attribute

- Format:

    /`$ldap_attrdesc(\s*,\s*$ldap_attrdesc)?`/

- Default:

    `mail`

#### `include_ldap_2level_query.select2`

second-level selection

- Format:
    - `all` - all
    - `first` - first entry
    - `regex` - entries matching regular expression
- Default:

    `first`

#### `include_ldap_2level_query.regex2`

second-level regular expression

- Format:

    /`.+`/

- Default:

    ``

#### `include_ldap_2level_query.nosync_time_ranges`

Time ranges when inclusion is not allowed

- Format:

    /`$time_ranges`/

- Default:

    None.

#### `include_ldap_2level_query.use_ssl`

Obsoleted. Use [`use_tls`](#include_ldap_2level_queryuse_tls).

### `include_sql_query`

SQL query inclusion

- Multiple occurrences allowed

This parameter is used to define the SQL query parameters. 

#### `include_sql_query.name`

short name for this source

- Format:

    /`.+`/

- Default:

    None.

#### `include_sql_query.db_type`

database type

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `include_sql_query.host`

remote host

- Format:

    /`$host`/

- Default:

    None.

#### `include_sql_query.db_port`

database port

- Format:

    /`\d+`/

- Default:

    None.

#### `include_sql_query.connect_options`

connection options

- Format:

    /`.+`/

- Default:

    None.

#### `include_sql_query.db_name`

database name

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `include_sql_query.db_env`

environment variables for database connection

- Format:

    /`\w+\=\S+(;\w+\=\S+)*`/

- Default:

    None.

#### `include_sql_query.user`

remote user

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `include_sql_query.passwd`

remote password

- Format:

    The value to be concealed.

- Default:

    None.

#### `include_sql_query.sql_query`

SQL query

- Format:

    /`$sql_query`/

- Default:

    None, _mandatory_.

#### `include_sql_query.f_dir`

Directory where the database is stored (used for DBD::CSV only)

- Format:

    /`.+`/

- Default:

    None.

#### `include_sql_query.nosync_time_ranges`

Time ranges when inclusion is not allowed

- Format:

    /`$time_ranges`/

- Default:

    None.

### `include_voot_group`

VOOT group inclusion

- Multiple occurrences allowed

#### `include_voot_group.name`

short name for this source

- Format:

    /`.+`/

- Default:

    None.

#### `include_voot_group.user`

user

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `include_voot_group.provider`

provider

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `include_voot_group.group`

group

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

### `ttl`

Inclusions timeout

- Format:

    Number of seconds.

- Default:

    `3600`

Sympa caches user data extracted using the include parameter. Their TTL (time-to-live) within Sympa can be controlled using this parameter. The default value is 3600

### `distribution_ttl`

Inclusions timeout for message distribution

- Format:

    Number of seconds.

- Default:

    None.

This parameter defines the delay since the last synchronization after which the user's list will be updated before performing either of following actions:

\* Reviewing list members

\* Message distribution

### `include_ldap_ca`

LDAP query custom attribute

- Multiple occurrences allowed

#### `include_ldap_ca.name`

short name for this source

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_ca.host`

remote host

- Format:

    /`$multiple_host_or_url`/

- Default:

    None, _mandatory_.

#### `include_ldap_ca.port`

Deprecated.

#### `include_ldap_ca.use_tls`

use TLS (formerly SSL)

- Format:
    - `starttls` - use STARTTLS
    - `ldaps` - use LDAPS (LDAP over TLS)
    - `none` - do nothing
- Default:

    `none`

#### `include_ldap_ca.ssl_version`

SSL version

- Format:
    - `sslv2` - SSL version 2
    - `sslv3` - SSL version 3
    - `tlsv1` - TLS version 1
    - `tlsv1_1` - TLS version 1.1
    - `tlsv1_2` - TLS version 1.2
- Default:

    `tlsv1`

#### `include_ldap_ca.ssl_ciphers`

SSL ciphers used

- Format:

    /`.+`/

- Default:

    `ALL`

#### `include_ldap_ca.ca_verify`

Certificate verification

- Format:
    - `none` - do nothing
    - `optional` - optional
    - `required` - required
- Default:

    `required`

#### `include_ldap_ca.user`

remote user

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_ca.passwd`

remote password

- Format:

    The value to be concealed.

- Default:

    None.

#### `include_ldap_ca.suffix`

suffix

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_ca.scope`

search scope

- Format:
    - `base` - base
    - `one` - one level
    - `sub` - subtree
- Default:

    `sub`

#### `include_ldap_ca.timeout`

connection timeout

- Format:

    Number of seconds.

- Default:

    `30`

#### `include_ldap_ca.filter`

filter

- Format:

    /`.+`/

- Default:

    None, _mandatory_.

#### `include_ldap_ca.attrs`

extracted attribute

- Format:

    /`$ldap_attrdesc`/

- Default:

    `mail`

#### `include_ldap_ca.email_entry`

Name of email entry

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `include_ldap_ca.select`

selection (if multiple)

- Format:
    - `all` - all
    - `first` - first entry
- Default:

    `first`

#### `include_ldap_ca.nosync_time_ranges`

Time ranges when inclusion is not allowed

- Format:

    /`$time_ranges`/

- Default:

    None.

#### `include_ldap_ca.use_ssl`

Obsoleted. Use [`use_tls`](#include_ldap_cause_tls).

### `include_ldap_2level_ca`

LDAP 2-level query custom attribute

- Multiple occurrences allowed

#### `include_ldap_2level_ca.host`

remote host

- Format:

    /`$multiple_host_or_url`/

- Default:

    None, _mandatory_.

#### `include_ldap_2level_ca.name`

short name for this source

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_2level_ca.port`

Deprecated.

#### `include_ldap_2level_ca.use_tls`

use TLS (formerly SSL)

- Format:
    - `starttls` - use STARTTLS
    - `ldaps` - use LDAPS (LDAP over TLS)
    - `none` - do nothing
- Default:

    `none`

#### `include_ldap_2level_ca.ssl_version`

SSL version

- Format:
    - `sslv2` - SSL version 2
    - `sslv3` - SSL version 3
    - `tlsv1` - TLS version 1
    - `tlsv1_1` - TLS version 1.1
    - `tlsv1_2` - TLS version 1.2
- Default:

    `tlsv1`

#### `include_ldap_2level_ca.ssl_ciphers`

SSL ciphers used

- Format:

    /`.+`/

- Default:

    `ALL`

#### `include_ldap_2level_ca.ca_verify`

Certificate verification

- Format:
    - `none` - do nothing
    - `optional` - optional
    - `required` - required
- Default:

    `required`

#### `include_ldap_2level_ca.user`

remote user

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_2level_ca.passwd`

remote password

- Format:

    The value to be concealed.

- Default:

    None.

#### `include_ldap_2level_ca.suffix1`

first-level suffix

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_2level_ca.scope1`

first-level search scope

- Format:
    - `base` - base
    - `one` - one level
    - `sub` - subtree
- Default:

    `sub`

#### `include_ldap_2level_ca.timeout1`

first-level connection timeout

- Format:

    Number of seconds.

- Default:

    `30`

#### `include_ldap_2level_ca.filter1`

first-level filter

- Format:

    /`.+`/

- Default:

    None, _mandatory_.

#### `include_ldap_2level_ca.attrs1`

first-level extracted attribute

- Format:

    /`$ldap_attrdesc`/

- Default:

    None.

#### `include_ldap_2level_ca.select1`

first-level selection

- Format:
    - `all` - all
    - `first` - first entry
    - `regex` - entries matching regular expression
- Default:

    `first`

#### `include_ldap_2level_ca.regex1`

first-level regular expression

- Format:

    /`.+`/

- Default:

    ``

#### `include_ldap_2level_ca.suffix2`

second-level suffix template

- Format:

    /`.+`/

- Default:

    None.

#### `include_ldap_2level_ca.scope2`

second-level search scope

- Format:
    - `base` - base
    - `one` - one level
    - `sub` - subtree
- Default:

    `sub`

#### `include_ldap_2level_ca.timeout2`

second-level connection timeout

- Format:

    Number of seconds.

- Default:

    `30`

#### `include_ldap_2level_ca.filter2`

second-level filter template

- Format:

    /`.+`/

- Default:

    None, _mandatory_.

#### `include_ldap_2level_ca.attrs2`

second-level extracted attribute

- Format:

    /`$ldap_attrdesc`/

- Default:

    `mail`

#### `include_ldap_2level_ca.select2`

second-level selection

- Format:
    - `all` - all
    - `first` - first entry
    - `regex` - entries matching regular expression
- Default:

    `first`

#### `include_ldap_2level_ca.regex2`

second-level regular expression

- Format:

    /`.+`/

- Default:

    ``

#### `include_ldap_2level_ca.email_entry`

Name of email entry

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `include_ldap_2level_ca.nosync_time_ranges`

Time ranges when inclusion is not allowed

- Format:

    /`$time_ranges`/

- Default:

    None.

#### `include_ldap_2level_ca.use_ssl`

Obsoleted. Use [`use_tls`](#include_ldap_2level_cause_tls).

### `include_sql_ca`

SQL query custom attribute

- Multiple occurrences allowed

#### `include_sql_ca.name`

short name for this source

- Format:

    /`.+`/

- Default:

    None.

#### `include_sql_ca.db_type`

database type

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `include_sql_ca.host`

remote host

- Format:

    /`$host`/

- Default:

    None, _mandatory_.

#### `include_sql_ca.db_port`

database port

- Format:

    /`\d+`/

- Default:

    None.

#### `include_sql_ca.db_name`

database name

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `include_sql_ca.connect_options`

connection options

- Format:

    /`.+`/

- Default:

    None.

#### `include_sql_ca.db_env`

environment variables for database connection

- Format:

    /`\w+\=\S+(;\w+\=\S+)*`/

- Default:

    None.

#### `include_sql_ca.user`

remote user

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `include_sql_ca.passwd`

remote password

- Format:

    The value to be concealed.

- Default:

    None.

#### `include_sql_ca.sql_query`

SQL query

- Format:

    /`$sql_query`/

- Default:

    None, _mandatory_.

#### `include_sql_ca.f_dir`

Directory where the database is stored (used for DBD::CSV only)

- Format:

    /`.+`/

- Default:

    None.

#### `include_sql_ca.email_entry`

Name of email entry

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `include_sql_ca.nosync_time_ranges`

Time ranges when inclusion is not allowed

- Format:

    /`$time_ranges`/

- Default:

    None.

### `include_list`

Deprecated.

### `user_data_source`

Deprecated.

## DKIM

### `dkim_feature`

Insert DKIM signature to messages sent to the list

- Format:
    - `on` - enabled
    - `off` - disabled
- Default:

    Value of [`dkim_feature`](./sympa.conf.5.md#dkim_feature) parameter in `sympa.conf` or `robot.conf`.

Enable/Disable DKIM. This feature require Mail::DKIM to installed and may be some custom scenario to be updated

### `dkim_parameters`

DKIM configuration

- Single occurrence

A set of parameters in order to define outgoing DKIM signature

#### `dkim_parameters.private_key_path`

File path for list DKIM private key

- Format:

    /`\S+`/

- Default:

    Value of [`dkim_private_key_path`](./sympa.conf.5.md#dkim_private_key_path) parameter in `sympa.conf` or `robot.conf`.

The file must contain a RSA pem encoded private key

#### `dkim_parameters.selector`

Selector for DNS lookup of DKIM public key

- Format:

    /`\S+`/

- Default:

    Value of [`dkim_selector`](./sympa.conf.5.md#dkim_selector) parameter in `sympa.conf` or `robot.conf`.

The selector is used in order to build the DNS query for public key. It is up to you to choose the value you want but verify that you can query the public DKIM key for &lt;selector>.\_domainkey.your\_domain

#### `dkim_parameters.header_list`

Deprecated.

#### `dkim_parameters.signer_domain`

DKIM "d=" tag, you should probably use the default value

- Format:

    /`\S+`/

- Default:

    Value of [`dkim_signer_domain`](./sympa.conf.5.md#dkim_signer_domain) parameter in `sympa.conf` or `robot.conf`.

The DKIM "d=" tag, is the domain of the signing entity. the list domain MUST be included in the "d=" domain

#### `dkim_parameters.signer_identity`

DKIM "i=" tag, you should probably leave this parameter empty

- Format:

    /`\S+`/

- Default:

    None.

DKIM "i=" tag, you should probably not use this parameter, as recommended by RFC 4871, default for list brodcasted messages is i=&lt;listname>-request@&lt;domain>

### `dkim_signature_apply_on`

The categories of messages sent to the list that will be signed using DKIM.

- Format:

    Multiple values allowed, separated by `,`.

    - `md5_authenticated_messages` - authenticated by password
    - `smime_authenticated_messages` - authenticated by S/MIME signature
    - `dkim_authenticated_messages` - authenticated by DKIM signature
    - `editor_validated_messages` - approved by editor
    - `none` - do nothing
    - `any` - any messages

- Default:

    Value of [`dkim_signature_apply_on`](./sympa.conf.5.md#dkim_signature_apply_on) parameter in `sympa.conf` or `robot.conf`.

This parameter controls in which case messages must be signed using DKIM, you may sign every message choosing 'any' or a subset. The parameter value is a comma separated list of keywords

### `dmarc_protection`

DMARC Protection

- Single occurrence

Parameters to define how to manage From address processing to avoid some domains' excessive DMARC protection

#### `dmarc_protection.mode`

Protection modes

- Format:

    Multiple values allowed, separated by `,`.

    - `none` - do nothing
    - `all` - all
    - `dkim_signature` - DKIM signature exists
    - `dmarc_reject` - DMARC policy suggests rejection
    - `dmarc_any` - DMARC policy exists
    - `dmarc_quarantine` - DMARC policy suggests quarantine
    - `domain_regex` - domain matching regular expression

- Default:

    Value of [`dmarc_protection_mode`](./sympa.conf.5.md#dmarc_protection_mode) parameter in `sympa.conf` or `robot.conf`.

Select one or more operation modes.  "Domain matching regular expression" (domain\_regex) matches the specified Domain regexp; "DKIM signature exists" (dkim\_signature) matches any message with a DKIM signature header; "DMARC policy ..." (dmarc\_\*) matches messages from sender domains with a DMARC policy as given; "all" (all) matches all messages.

#### `dmarc_protection.domain_regex`

Match domain regexp

- Format:

    /`.+`/

- Default:

    Value of [`dmarc_protection_domain_regex`](./sympa.conf.5.md#dmarc_protection_domain_regex) parameter in `sympa.conf` or `robot.conf`.

Regexp match pattern for From domain

#### `dmarc_protection.other_email`

New From address

- Format:

    /`.+`/

- Default:

    Value of [`dmarc_protection_other_email`](./sympa.conf.5.md#dmarc_protection_other_email) parameter in `sympa.conf` or `robot.conf`.

This is the email address to use when modifying the From header.  It defaults to the list address.  This is similar to Anonymisation but preserves the original sender details in the From address phrase.

#### `dmarc_protection.phrase`

New From name format

- Format:
    - `display_name` - "Name"
    - `name_and_email` - "Name" (e-mail)
    - `name_via_list` - "Name" (via List)
    - `name_email_via_list` - "Name" (e-mail via List)
    - `list_for_email` - "List" (on behalf of e-mail)
    - `list_for_name` - "List" (on behalf of Name)
- Default:

    Value of [`dmarc_protection_phrase`](./sympa.conf.5.md#dmarc_protection_phrase) parameter in `sympa.conf` or `robot.conf`.

This is the format to be used for the sender name part of the new From header.

## Miscellaneous

### `clean_delay_queuemod`

Expiration of unmoderated messages

- Format:

    Number of days.

- Default:

    Value of [`clean_delay_queuemod`](./sympa.conf.5.md#clean_delay_queuemod) parameter in `sympa.conf`.

### `cookie`

Secret string for generating unique keys

- Format:

    The value to be concealed.

- Default:

    Value of [`cookie`](./sympa.conf.5.md#cookie) parameter in `sympa.conf`.

This parameter is a confidential item for generating authentication keys for administrative commands (ADD, DELETE, etc.). This parameter should remain concealed, even for owners. The cookie is applied to all list owners, and is only taken into account when the owner has the auth parameter.

### `custom_attribute`

Custom user attributes

- Multiple occurrences allowed

#### `custom_attribute.id`

internal identifier

- Format:

    /`\w+`/

- Default:

    None, _mandatory_.

#### `custom_attribute.name`

label

- Format:

    /`.+`/

- Default:

    None, _mandatory_.

#### `custom_attribute.comment`

additional comment

- Format:

    /`.+`/

- Default:

    None.

#### `custom_attribute.type`

type

- Format:
    - `string` - string
    - `text` - multi-line text
    - `integer` - number
    - `enum` - set of keywords
- Default:

    `string`

#### `custom_attribute.enum_values`

possible attribute values (if enum is used)

- Format:

    /`.+`/

- Default:

    None.

#### `custom_attribute.optional`

is the attribute optional?

- Format:
    - `required` - required
    - `optional` - optional
- Default:

    `optional`

### `custom_vars`

custom parameters

- Multiple occurrences allowed

#### `custom_vars.name`

var name

- Format:

    /`\S+`/

- Default:

    None, _mandatory_.

#### `custom_vars.value`

var value

- Format:

    /`.+`/

- Default:

    None, _mandatory_.

### `expire_task`

Periodical subscription expiration task

- Format:

    /`\w+`/

- Default:

    None.

This parameter states which model is used to create an expire task. An expire task regularly checks the subscription or resubscription  date of subscribers and asks them to renew their subscription. If they don't they are deleted.

### `loop_prevention_regex`

Regular expression applied to prevent loops with robots

- Format:

    /`\S*`/

- Default:

    Value of [`loop_prevention_regex`](./sympa.conf.5.md#loop_prevention_regex) parameter in `sympa.conf` or `robot.conf`.

### `pictures_feature`

Allow picture display? (must be enabled for the current robot)

- Format:
    - `on` - enabled
    - `off` - disabled
- Default:

    Value of [`pictures_feature`](./sympa.conf.5.md#pictures_feature) parameter in `sympa.conf`.

### `remind_task`

Periodical subscription reminder task

- Format:

    /`\w+`/

- Default:

    Value of [`default_remind_task`](./sympa.conf.5.md#default_remind_task) parameter in `sympa.conf`.

This parameter states which model is used to create a remind task. A remind task regularly sends  subscribers a message which reminds them of their list subscriptions.

### `spam_protection`

email address protection method

- Format:
    - `at` - replace @ characters
    - `javascript` - use JavaScript
    - `none` - do nothing
- Default:

    `javascript`

There is a need to protect Sympa web sites against spambots which collect email addresses from public web sites. Various methods are available in Sympa and you can choose to use them with the spam\_protection and web\_archive\_spam\_protection parameters. Possible value are:

javascript: 

the address is hidden using a javascript. A user who enables javascript can see a nice mailto address where others have nothing.

at: 

the @ char is replaced by the string " AT ".

none: 

no protection against spammer.

### `latest_instantiation`

Latest family instantiation

- Single occurrence

#### `latest_instantiation.email`

who ran the instantiation

- Format:

    /`listmaster|$email`/

- Default:

    None.

#### `latest_instantiation.date_epoch`

date

- Format:

    The time in second from Unix epoch.

- Default:

    None, _mandatory_.

#### `latest_instantiation.date`

Deprecated.

### `creation`

Creation of the list

- Single occurrence

#### `creation.email`

who created the list

- Format:

    /`listmaster|$email`/

- Default:

    None, _mandatory_.

#### `creation.date_epoch`

date

- Format:

    The time in second from Unix epoch.

- Default:

    None, _mandatory_.

#### `creation.date`

Deprecated.

### `update`

Last update of config

- Single occurrence

#### `update.email`

who updated the config

- Format:

    /`(listmaster|automatic|$email)`/

- Default:

    None.

#### `update.date_epoch`

date

- Format:

    The time in second from Unix epoch.

- Default:

    None, _mandatory_.

#### `update.date`

Deprecated.

### `status`

Status of the list

- Format:

    Status of list.

- Default:

    `open`

### `serial`

Serial number of the config

- Format:

    /`\d+`/

- Default:

    `0`

### `account`

Deprecated.

# FILES

- `$EXPLDIR/<list name>/config`
or `$EXPLDIR/<mail domain name>/<list name>/config`

    List main configuration file.

# SEE ALSO

[sympa.conf(5)](./sympa.conf.5.md).

_Sympa, Mailing List Management Software - Reference Manual_.
[http://www.sympa.org/manual/](http://www.sympa.org/manual/).
