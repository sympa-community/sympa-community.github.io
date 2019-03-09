---
title: 'sympa.conf(5)'
---

# NAME

sympa.conf, robot.conf - Configuration file for default site and robot

# DESCRIPTION

`/etc/sympa/sympa.conf` is main configuration file of Sympa.
Several parameters defined in this file may be overridden by `robot.conf`
configuration file for each virtual domain, or by `config` configuration file
for each mailing list.

Format of `sympa.conf` and `robot.conf` is as following:

- Lines beginning with `#` and containing only spaces are ignored.
- Each line has the form "_parameter_ _value_".
_value_ may contain spaces but may not contain newlines.

# PARAMETERS

Below is entire list of configuration parameters.
"Default" is built-in default value if any.
"Overrides" lists contexts (with parameter name) which can override
settings in site-wide context (`sympa.conf`): Virtual domain (`robot.conf`)
and/or List (`config`).

## Service description

#### `domain`

Primary mail domain name

- Default:

    None, _mandatory_.

- Overrides:

    Virtual domain

Example:

    domain mail.example.org

#### `listmaster`

Email addresses of listmasters

- Default:

    None, _mandatory_.

- Overrides:

    Virtual domain

Email addresses of the listmasters (users authorized to perform global server commands). Some error reports may also be sent to these addresses. Listmasters can be defined for each virtual host, however, the default listmasters will have privileges to manage all virtual hosts.

Example:

    listmaster your_email_address@domain.tld

#### `lang`

Default language

- Default:

    `en-US`

- Overrides:

    Virtual domain

    List

This is the default language used by Sympa. One of supported languages should be chosen.

#### `supported_lang`

Supported languages

- Default:

    `ca,cs,de,el,en-US,es,et,eu,fi,fr,gl,hu,it,ja,ko,nb,nl,oc,pl,pt-BR,ru,sv,tr,vi,zh-CN,zh-TW`

- Overrides:

    Virtual domain

All supported languages for the user interface. Languages proper locale information not installed are ignored.

#### `title`

Title of service

- Default:

    `Mailing lists service`

- Overrides:

    Virtual domain

The name of your mailing list service. It will appear in the header of web interface and subjects of several service messages.

#### `gecos`

Display name of Sympa

- Default:

    `SYMPA`

- Overrides:

    Virtual domain

This parameter is used for display name in the "From:" header field for the messages sent by Sympa itself.

#### `legacy_character_support_feature`

Support of legacy character set

- Default:

    `off`

- Overrides:

    None.

If set to "on", enables support of legacy character set according to charset.conf(5) configuration file.

In some language environments, legacy encoding (character set) can be preferred for e-mail messages: for example iso-2022-jp in Japanese language.

## Database related

#### `update_db_field_types`

Update database structure

- Default:

    `auto`

- Overrides:

    None.

auto: Updates database table structures automatically.

However, since version 5.3b.5, Sympa will not shorten field size if it already have been longer than the size defined in database definition.

#### `db_type`

Type of the database

- Default:

    `mysql`

- Overrides:

    None.

Possible types are "MySQL", "PostgreSQL", "Oracle" and "SQLite".

#### `db_host`

Hostname of the database server

- Default:

    None.

- Overrides:

    None.

With PostgreSQL, you can also use the path to Unix Socket Directory, e.g. "/var/run/postgresql" for connection with Unix domain socket.

Example:

    db_host localhost

#### `db_port`

Port of the database server

- Default:

    None.

- Overrides:

    None.

#### `db_name`

Name of the database

- Default:

    `sympa`

- Overrides:

    None.

With SQLite, this must be the full path to database file.

With Oracle Database, this must be SID, net service name or easy connection identifier (to use net service name, db\_host should be set to "none" and HOST, PORT and SERVICE\_NAME should be defined in tnsnames.ora file).

#### `db_user`

User for the database connection

- Default:

    `user_name`

- Overrides:

    None.

Example:

    db_user sympa

#### `db_passwd`

Password for the database connection

- Default:

    `user_password`

- Overrides:

    None.

What ever you use a password or not, you must protect the SQL server (is it not a public internet service ?)

Example:

    db_passwd your_passwd

#### `db_options`

Database options

- Default:

    None.

- Overrides:

    None.

If these options are defined, they will be appended to data source name (DSN) fed to database driver. Check the related DBD documentation to learn about the available options.

Example:

    db_options mysql_read_default_file=/home/joe/my.cnf;mysql_socket=tmp/mysql.sock-test

#### `db_env`

Environment variables setting for database

- Default:

    None.

- Overrides:

    None.

With Oracle Database, this is useful for defining ORACLE\_HOME and NLS\_LANG.

Example:

    db_env NLS_LANG=American_America.AL32UTF8;ORACLE_HOME=/u01/app/oracle/product/11.2.0/server

#### `db_timeout`

Database processing timeout

- Default:

    None.

- Overrides:

    None.

Currently, this parameter may be used for SQLite only.

#### `db_additional_subscriber_fields`

Database private extension to subscriber table

- Default:

    None.

- Overrides:

    None.

Adds more fields to "subscriber\_table" table. Sympa recognizes fields defined with this parameter. You will then be able to use them from within templates and scenarios:

\* for scenarios: \[subscriber->field\]

\* for templates: \[% subscriber.field %\]

These fields will also appear in the list members review page and will be editable by the list owner. This parameter is a comma-separated list.

You need to extend the database format with these fields

Example:

    db_additional_subscriber_fields billing_delay,subscription_expiration

#### `db_additional_user_fields`

Database private extension to user table

- Default:

    None.

- Overrides:

    None.

Adds more fields to "user\_table" table. Sympa recognizes fields defined with this parameter. You will then be able to use them from within templates: \[% subscriber.field %\]

This parameter is a comma-separated list.

You need to extend the database format with these fields

Example:

    db_additional_user_fields age,address

## System log

#### `syslog`

System log facility for Sympa

- Default:

    `LOCAL1`

- Overrides:

    None.

Do not forget to configure syslog server.

#### `log_socket_type`

Communication mode with syslog server

- Default:

    `unix`

- Overrides:

    None.

#### `log_level`

Log verbosity

- Default:

    `0`

- Overrides:

    Virtual domain

Sets the verbosity of logs.

0: Only main operations are logged

3: Almost everything is logged.

Example:

    log_level 2

## Alias management

#### `aliases_program`

Program used to update alias database

- Default:

    `newaliases`

- Overrides:

    Virtual domain

This may be "makemap", "newaliases", "postalias", "postmap" or full path to custom program.

#### `aliases_db_type`

Type of alias database

- Default:

    `hash`

- Overrides:

    Virtual domain

"btree", "dbm", "hash" and so on.  Available when aliases\_program is "makemap", "postalias" or "postmap"

#### `sendmail_aliases`

Path of the file that contains all list related aliases

- Default:

    `$SENDMAIL_ALIASES`

- Overrides:

    Virtual domain

It is recommended to create a specific alias file so that Sympa never overwrites the standard alias file, but only a dedicated file.

Set this parameter to "none" if you want to disable alias management in Sympa.

#### `alias_manager`

Path to alias manager

- Default:

    `$SBINDIR/alias_manager.pl`

- Overrides:

    None.

The absolute path to the script that will add/remove mail aliases

Example:

    alias_manager /usr/local/libexec/ldap_alias_manager.pl

## Receiving

#### `default_max_list_members`

Default maximum number of list members

- Default:

    `0`

- Overrides:

    Virtual domain

    List (`max_list_members`)

Default limit for the number of subscribers per list (0 means no limit).

#### `max_size`

Maximum size of messages

- Default:

    `5242880` (bytes)

- Overrides:

    Virtual domain

    List

Incoming messages smaller than this size is allowed distribution by Sympa.

Example:

    max_size 2097152

#### `reject_mail_from_automates_feature`

Reject mail sent from automated services to list

- Default:

    `on`

- Overrides:

    List

Rejects messages that seem to be from automated services, based on a few header fields ("Content-Identifier:", "Auto-Submitted:").

Sympa also can be configured to reject messages based on the "From:" header field value (see "loop\_prevention\_regex").

Example:

    reject_mail_from_automates_feature off

#### `sender_headers`

Header field name(s) used to determine sender of the messages

- Default:

    `From`

- Overrides:

    None.

"Return-Path" means envelope sender (a.k.a. "UNIX From") which will be alternative to sender of messages without "From" field.  "Resent-From" may also be inserted before "From", because some mailers add it into redirected messages and keep original "From" field intact.  In particular cases, "Return-Path" can not give right sender: several mail gateway products rewrite envelope sender and add original one as non-standard field such as "X-Envelope-From".  If that is the case, you might want to insert it in place of "Return-Path".

Example:

    sender_headers Resent-From,From,Return-Path

#### `misaddressed_commands`

Reject misaddressed commands

- Default:

    `reject`

- Overrides:

    None.

When a mail command is sent to a list, by default Sympa rejects this message. This feature can be turned off setting this parameter to "ignore".

#### `misaddressed_commands_regexp`

Regular expression matching with misaddressed commands

- Default:

    `((subscribe\s+(\S+)|unsubscribe\s+(\S+)|signoff\s+(\S+)|set\s+(\S+)\s+(mail|nomail|digest))\s*)`

- Overrides:

    None.

Perl regular expression applied on messages subject and body to detect misaddressed commands.

#### `sympa_priority`

Priority for command messages

- Default:

    `1`

- Overrides:

    Virtual domain

Priority applied to messages sent to Sympa command address.

#### `request_priority`

Priority for messages bound for list owners

- Default:

    `0`

- Overrides:

    Virtual domain

Priority for processing of messages bound for "LIST-request" address, i.e. owners of the list

#### `owner_priority`

Priority for non-VERP bounces

- Default:

    `9`

- Overrides:

    Virtual domain

Priority for processing of messages bound for "LIST-owner" address, i.e. non-delivery reports (bounces).

#### `default_list_priority`

Default priority for list messages

- Default:

    `5`

- Overrides:

    Virtual domain

    List (`priority`)

Priority for processing of messages posted to list addresses.

#### `incoming_max_count`

Max number of sympa.pl workers

- Default:

    `1`

- Overrides:

    None.

Max number of workers of sympa.pl daemon processing incoming spool.

#### `sleep`

Interval between scanning incoming message spool

- Default:

    `5` (seconds)

- Overrides:

    None.

Must not be 0.

## Sending related

#### `anonymous_header_fields`

Header fields removed when a mailing list is setup in anonymous mode

- Default:

    `Authentication-Results,Disposition-Notification-To,DKIM-Signature,Injection-Info,Organisation,Organization,Original-Recipient,Originator,Path,Received,Received-SPF,Reply-To,Resent-Reply-To,Return-Receipt-To,X-Envelope-From,X-Envelope-To,X-Sender,X-X-Sender`

- Overrides:

    None.

See "anonymous\_sender" list parameter.

Default value prior to Sympa 6.1.19 is:

    Sender,X-Sender,Received,Message-id,From,X-Envelope-To,Resent-From,Reply-To,Organization,Disposition-Notification-To,X-Envelope-From,X-X-Sender

#### `merge_feature`

Allow message personalization by default

- Default:

    `off`

- Overrides:

    List

This parameter defines the default "merge\_feature" list parameter.

#### `remove_headers`

Header fields to be removed from incoming messages

- Default:

    `X-Sympa-To,X-Family-To,Return-Receipt-To,Precedence,X-Sequence,Disposition-Notification-To,Sender`

- Overrides:

    List

Use it, for example, to ensure some privacy for your users in case that "anonymous\_sender" mode is inappropriate.

The removal of these header fields is applied before Sympa adds its own header fields ("rfc2369\_header\_fields" and "custom\_header").

Example:

    remove_headers Resent-Date,Resent-From,Resent-To,Resent-Message-Id,Sender,Delivered-To

#### `remove_outgoing_headers`

Header fields to be removed before message distribution

- Default:

    `none`

- Overrides:

    List

The removal happens after Sympa's own header fields are added; therefore, it is a convenient way to remove Sympa's own header fields (like "X-Loop:" or "X-no-archive:") if you wish.

Example:

    remove_outgoing_headers X-no-archive

#### `rfc2369_header_fields`

RFC 2369 header fields

- Default:

    `help,subscribe,unsubscribe,post,owner,archive`

- Overrides:

    List

Specify which RFC 2369 mailing list header fields to be added.

"List-Id:" header field defined in RFC 2919 is always added. Sympa also adds "Archived-At:" header field defined in RFC 5064.

#### `urlize_min_size`

Minimum size to be urlized

- Default:

    `10240` (bytes)

- Overrides:

    Virtual domain

When a subscriber chose "urlize" reception mode, attachments not smaller than this size will be urlized.

#### `allowed_external_origin`

Allowed external links in sanitized HTML

- Default:

    None.

- Overrides:

    Virtual domain

When the HTML content of a message must be sanitized, links ("href" or "src" attributes) with the hosts listed in this parameter will not be scrubbed. If "\*" character is included, it matches any subdomains. Single "\*" allows any hosts.

Example:

    allowed_external_origin *.example.org,www.example.com

#### `sympa_packet_priority`

Default priority for a packet

- Default:

    `5`

- Overrides:

    Virtual domain

The default priority set to a packet to be sent by the bulk.

#### `bulk_fork_threshold`

Fork threshold of bulk daemon

- Default:

    `1`

- Overrides:

    None.

The minimum number of packets before bulk daemon forks a new worker to increase sending rate.

#### `bulk_max_count`

Maximum number of bulk workers

- Default:

    `3`

- Overrides:

    None.

#### `bulk_lazytime`

Idle timeout of bulk workers

- Default:

    `600` (seconds)

- Overrides:

    None.

The number of seconds a bulk worker will remain running without processing a message before it spontaneously exits.

#### `bulk_sleep`

Sleep time of bulk workers

- Default:

    `1` (seconds)

- Overrides:

    None.

The number of seconds a bulk worker sleeps between starting a new loop if it didn't find a message to send.

Keep it small if you want your server to be reactive.

#### `bulk_wait_to_fork`

Interval between checks of packet numbers

- Default:

    `10` (seconds)

- Overrides:

    None.

Number of seconds a master bulk daemon waits between two packets number checks.

Keep it small if you expect brutal increases in the message sending load.

#### `sendmail`

Path to sendmail

- Default:

    `/usr/sbin/sendmail`

- Overrides:

    None.

Absolute path to sendmail command line utility (e.g.: a binary named "sendmail" is distributed with Postfix).

Sympa expects this binary to be sendmail compatible (exim, Postfix, qmail and so on provide it).

#### `sendmail_args`

Command line parameters passed to sendmail

- Default:

    `-oi -odi -oem`

- Overrides:

    None.

Note that "-f", "-N" and "-V" options and recipient addresses should not be included, because they will be included by Sympa.

#### `log_smtp`

Log invocation of sendmail

- Default:

    `off`

- Overrides:

    Virtual domain

This can be overwritten by "-m" option for sympa.pl.

#### `maxsmtp`

Maximum number of sendmail processes

- Default:

    `40`

- Overrides:

    None.

Maximum number of simultaneous child processes spawned by Sympa. This is the main load control parameter. 

Proposed value is quite low, but you can rise it up to 100, 200 or even 300 with powerful systems.

Example:

    maxsmtp 500

#### `nrcpt`

Maximum number of recipients per call to sendmail

- Default:

    `25`

- Overrides:

    None.

This grouping factor makes it possible for the sendmail processes to optimize the number of SMTP sessions for message distribution. If needed, you can limit the number of recipients for a particular domain. Check the "nrcpt\_by\_domain.conf" configuration file.

#### `avg`

Maximum number of different mail domains per call to sendmail

- Default:

    `10`

- Overrides:

    None.

## Privileges

#### `create_list`

Who is able to create lists

- Default:

    `public_listmaster`

- Overrides:

    Virtual domain

Value of this parameter is name of `create_list` scenario.

Defines who can create lists (or request list creation) by creating new lists or by renaming or copying existing lists.

Example:

    create_list intranet

#### `allow_subscribe_if_pending`

Allow adding subscribers to a list not open

- Default:

    `on`

- Overrides:

    Virtual domain

If set to "off", adding subscribers to, or removing subscribers from a list with status other than "open" is forbidden.

#### `global_remind`

Who is able to send remind messages over all lists

- Default:

    `listmaster`

- Overrides:

    None.

Value of this parameter is name of `global_remind` scenario.

#### `move_user`

Who is able to change user's email

- Default:

    `auth`

- Overrides:

    Virtual domain

Value of this parameter is name of `move_user` scenario.

#### `use_blacklist`

Use blacklist

- Default:

    `send,create_list`

- Overrides:

    Virtual domain

List of operations separated by comma for which blacklist filter is applied.  Setting this parameter to "none" will hide the blacklist feature.

#### `owner_domain`

List of required domains for list owner addresses

- Default:

    None.

- Overrides:

    Virtual domain

    List

Restrict list ownership to addresses in the specified domains. This can be used to reserve list ownership to a group of trusted users from a set of domains associated with an organization, while allowing moderators and subscribers from the Internet at large.

Example:

    owner_domain domain1.tld domain2.tld

#### `owner_domain_min`

Minimum number of owners for each list that must match owner\_domain restriction

- Default:

    `0`

- Overrides:

    Virtual domain

    List

Minimum number of owners for each list must satisfy the owner\_domain restriction. The default of zero (0) means \*all\* list owners must match. Setting to 1 requires only one list owner to match owner\_domain; all other owners can be from any domain. This setting can be used to ensure that there is always at least one known contact point for any mailing list.

Example:

    owner_domain_min 1

## Default privileges for the lists

#### `visibility`

Visibility of the list

- Default:

    `conceal`

- Overrides:

    Virtual domain

    List

Value of this parameter is name of `visibility` scenario.

#### `send`

Who can send messages

- Default:

    `private`

- Overrides:

    Virtual domain

    List

Value of this parameter is name of `send` scenario.

#### `info`

Who can view list information

- Default:

    `open`

- Overrides:

    Virtual domain

    List

Value of this parameter is name of `info` scenario.

#### `subscribe`

Who can subscribe to the list

- Default:

    `open`

- Overrides:

    Virtual domain

    List

Value of this parameter is name of `subscribe` scenario.

#### `add`

Who can add subscribers

- Default:

    `owner`

- Overrides:

    Virtual domain

    List

Value of this parameter is name of `add` scenario.

#### `unsubscribe`

Who can unsubscribe

- Default:

    `open`

- Overrides:

    Virtual domain

    List

Value of this parameter is name of `unsubscribe` scenario.

#### `del`

Who can delete subscribers

- Default:

    `owner`

- Overrides:

    Virtual domain

    List

Value of this parameter is name of `del` scenario.

#### `invite`

Who can invite people

- Default:

    `private`

- Overrides:

    Virtual domain

    List

Value of this parameter is name of `invite` scenario.

#### `remind`

Who can start a remind process

- Default:

    `owner`

- Overrides:

    Virtual domain

    List

Value of this parameter is name of `remind` scenario.

#### `review`

Who can review subscribers

- Default:

    `owner`

- Overrides:

    Virtual domain

    List

Value of this parameter is name of `review` scenario.

#### `d_read`

Who can view

- Default:

    `private`

- Overrides:

    Virtual domain

    List (`shared_doc.d_read`)

Value of this parameter is name of `d_read` scenario.

#### `d_edit`

Who can edit

- Default:

    `owner`

- Overrides:

    Virtual domain

    List (`shared_doc.d_edit`)

Value of this parameter is name of `d_edit` scenario.

#### `archive_web_access`

access right

- Default:

    `closed`

- Overrides:

    Virtual domain

    List (`archive.web_access`)

Value of this parameter is name of `archive_web_access` scenario.

#### `archive_mail_access`

access right by mail commands

- Default:

    `closed`

- Overrides:

    Virtual domain

    List (`archive.mail_access`)

Value of this parameter is name of `archive_mail_access` scenario.

#### `tracking`

who can view message tracking

- Default:

    `owner`

- Overrides:

    Virtual domain

    List (`tracking.tracking`)

Value of this parameter is name of `tracking` scenario.

## Archives

#### `process_archive`

Store distributed messages into archive

- Default:

    `off`

- Overrides:

    Virtual domain

    List

If enabled, distributed messages via lists will be archived. Otherwise archiving is disabled.

Note that even if setting this parameter disabled, past archives will not be removed and will be accessible according to access settings by each list.

#### `default_archive_quota`

Default disk quota for lists' archives

- Default:

    None (Kbytes).

- Overrides:

    List (`archive.quota`)

#### `ignore_x_no_archive_header_feature`

Ignore "X-no-archive:" header field

- Default:

    `off`

- Overrides:

    None.

Sympa's default behavior is to skip archiving of incoming messages that have an "X-no-archive:" header field set. This parameter allows one to change this behavior.

Example:

    ignore_x_no_archive_header_feature on

#### `custom_archiver`

Custom archiver

- Default:

    None.

- Overrides:

    None.

Activates a custom archiver to use instead of MHonArc. The value of this parameter is the absolute path to the executable file.

Sympa invokes this file with these two arguments:

\--list

The address of the list including domain part.

\--file

Absolute path to the message to be archived.

#### `mhonarc`

Path to MHonArc mail-to-HTML converter

- Default:

    `/usr/bin/mhonarc`

- Overrides:

    Virtual domain

This is required for HTML mail archiving.

## Bounce management and tracking

#### `bounce_warn_rate`

Default bounce warn rate

- Default:

    `30`

- Overrides:

    List (`bounce.warn_rate`)

The list owner receives a warning whenever a message is distributed and the number (percentage) of bounces exceeds this value.

#### `bounce_halt_rate`

Default bounce halt rate

- Default:

    `50`

- Overrides:

    None.

NOT USED YET. If bounce rate reaches the halt\_rate, messages for the list will be halted, i.e. they are retained for subsequent moderation.

#### `default_bounce_level1_rate`

Default bounce management threshold, 1st level

- Default:

    `45`

- Overrides:

    Virtual domain

    List (`bouncers_level1.rate`)

#### `default_bounce_level2_rate`

Default bounce management threshold, 2nd level

- Default:

    `75`

- Overrides:

    Virtual domain

    List (`bouncers_level2.rate`)

#### `verp_rate`

Percentage of list members in VERP mode

- Default:

    `0%`

- Overrides:

    Virtual domain

    List

Uses variable envelope return path (VERP) to detect bouncing subscriber addresses.

0%: VERP is never used.

100%: VERP is always in use.

VERP requires address with extension to be supported by MTA. If tracking is enabled for a list or a message, VERP is applied for 100% of subscribers.

#### `tracking_delivery_status_notification`

Tracking message by delivery status notification (DSN)

- Default:

    `off`

- Overrides:

    List (`tracking.delivery_status_notification`)

#### `tracking_message_disposition_notification`

Tracking message by message disposition notification (MDN)

- Default:

    `off`

- Overrides:

    List (`tracking.message_disposition_notification`)

#### `tracking_default_retention_period`

Max age of tracking information

- Default:

    `90` (days)

- Overrides:

    List (`tracking.retention_period`)

Tracking information is removed after this number of days

#### `welcome_return_path`

Remove bouncing new subscribers

- Default:

    `owner`

- Overrides:

    List

If set to unique, the welcome message is sent using a unique return path in order to remove the subscriber immediately in the case of a bounce.

#### `remind_return_path`

Remove subscribers bouncing remind message

- Default:

    `owner`

- Overrides:

    List

Same as welcome\_return\_path, but applied to remind messages.

#### `default_remind_task`

Periodical subscription reminder task

- Default:

    None.

- Overrides:

    List (`remind_task`)

This task regularly sends subscribers a message which reminds them of their list subscriptions.

#### `expire_bounce_task`

Task for expiration of old bounces

- Default:

    `daily`

- Overrides:

    None.

This task resets bouncing information for addresses not bouncing in the last 10 days after the latest message distribution.

#### `purge_orphan_bounces_task`

Task for cleaning invalidated bounces

- Default:

    `monthly`

- Overrides:

    None.

This task deletes bounce information for unsubscribed users.

#### `eval_bouncers_task`

Task for updating bounce scores

- Default:

    `daily`

- Overrides:

    None.

This task scans all bouncing users for all lists, and updates "bounce\_score\_subscriber" field in "subscriber\_table" table. The scores may be used for management of bouncers.

#### `process_bouncers_task`

Task for management of bouncers

- Default:

    `weekly`

- Overrides:

    None.

This task executes actions on bouncing users configured by each list, according to their scores.

#### `purge_tables_task`

Task for cleaning tables

- Default:

    `daily`

- Overrides:

    None.

This task cleans old tracking information from "notification\_table" table.

#### `minimum_bouncing_count`

Minimum number of bounces

- Default:

    `10`

- Overrides:

    None.

The minimum number of bounces received to update bounce score of a user.

#### `minimum_bouncing_period`

Minimum bouncing period

- Default:

    `10` (days)

- Overrides:

    None.

The minimum period for which bouncing lasted to update bounce score of a user.

#### `bounce_delay`

Delay of bounces

- Default:

    `0` (days)

- Overrides:

    None.

Average time for a bounce sent back to mailing list server after a post was sent to a list. Usually bounces are sent back on the same day as the original message.

#### `bounce_email_prefix`

- Default:

    `bounce`

- Overrides:

    None.

The prefix to consist the return-path of probe messages used for bounce management, when variable envelope return path (VERP) is enabled. VERP requires address with extension to be supported by MTA.

If you change the default value, you must modify the mail aliases too.

#### `return_path_suffix`

Suffix of list return address

- Default:

    `-owner`

- Overrides:

    None.

The suffix appended to the list name to form the return-path of messages distributed through the list. This address will receive all non-delivery reports (also called bounces).

## Loop prevention

#### `loop_command_max`

Maximum number of responses to command message

- Default:

    `200`

- Overrides:

    None.

The maximum number of command reports sent to an email address. Messages are stored in "bad" subdirectory of incoming message spool, and reports are not longer sent.

#### `loop_command_sampling_delay`

Delay before counting responses to command message

- Default:

    `3600` (seconds)

- Overrides:

    None.

This parameter defines the delay in seconds before decrementing the counter of reports sent to an email address.

#### `loop_command_decrease_factor`

Decrementing factor of responses to command message

- Default:

    `0.5`

- Overrides:

    None.

The decrementation factor (from 0 to 1), used to determine the new report counter after expiration of the delay.

#### `loop_prevention_regex`

Regular expression to prevent loop

- Default:

    `mailer-daemon|sympa|listserv|majordomo|smartlist|mailman`

- Overrides:

    Virtual domain

    List

If the sender address matches the regular expression, then the message is rejected.

#### `msgid_table_cleanup_ttl`

Expiration period of message ID table

- Default:

    `86400` (seconds)

- Overrides:

    None.

Expiration period of entries in the table maintained by sympa\_msg.pl daemon to prevent delivery of duplicate messages caused by loop.

#### `msgid_table_cleanup_frequency`

Cleanup interval of message ID table

- Default:

    `3600` (seconds)

- Overrides:

    None.

Interval between cleanups of the table maintained by sympa\_msg.pl daemon to prevent delivery of duplicate messages caused by loop.

## Automatic lists

#### `automatic_list_removal`

Remove empty automatic list

- Default:

    `none`

- Overrides:

    Virtual domain

If set to "if\_empty", then Sympa will remove automatically created mailing lists just after their creation, if they contain no list member.

Example:

    automatic_list_removal if_empty

#### `automatic_list_feature`

Automatic list

- Default:

    `off`

- Overrides:

    Virtual domain

#### `automatic_list_creation`

Who is able to create automatic list

- Default:

    `public`

- Overrides:

    Virtual domain

Value of this parameter is name of `automatic_list_creation` scenario.

#### `automatic_list_families`

Definition of automatic list families

- Default:

    None.

- Overrides:

    Virtual domain

Defines the families the automatic lists are based on. It is a character string structured as follows:

\* each family is separated from the other by a semicolon (;)

\* inside a family definition, each field is separated from the other by a colon (:)

\* each field has the structure: "&lt;field name>=&lt;field value>"

Basically, each time Sympa uses the automatic lists families, the values defined in this parameter will be available in the family object.

\* for scenarios: \[family->name\]

\* for templates: \[% family.name %\]

Example:

    automatic_list_families name=family_one:prefix=f1:display=My automatic lists:prefix_separator=+:classes separator=-:family_owners_list=alist@domain.tld;name=family_two:prefix=f2:display=My other automatic lists:prefix_separator=+:classes separator=-:family_owners_list=anotherlist@domain.tld;

#### `parsed_family_files`

Parsed files for families

- Default:

    `message_header,message_header.mime,message_footer,message_footer.mime,info`

- Overrides:

    Virtual domain

comma-separated list of files that will be parsed by Sympa when instantiating a family (no space allowed in file names)

## Tag based spam filtering

#### `antispam_feature`

Tag based spam filtering

- Default:

    `off`

- Overrides:

    Virtual domain

#### `antispam_tag_header_name`

Header field to tag spams

- Default:

    `X-Spam-Status`

- Overrides:

    Virtual domain

If a spam filter (like spamassassin or j-chkmail) add a header field to tag spams, name of this header field (example X-Spam-Status)

#### `antispam_tag_header_spam_regexp`

Regular expression to check header field to tag spams

- Default:

    `^\s*Yes`

- Overrides:

    Virtual domain

Regular expression applied on this header to verify message is a spam (example Yes)

#### `antispam_tag_header_ham_regexp`

Regular expression to determine spam or ham.

- Default:

    `^\s*No`

- Overrides:

    Virtual domain

Regular expression applied on this header field to verify message is NOT a spam (example No)

#### `spam_status`

Name of header field to inform

- Default:

    `x-spam-status`

- Overrides:

    Virtual domain

Value of this parameter is name of `spam_status` scenario.

Messages are supposed to be filtered by an spam filter that adds them one or more headers. This parameter is used to select a special scenario in order to decide the message's spam status: ham, spam or unsure. This parameter replaces antispam\_tag\_header\_name, antispam\_tag\_header\_spam\_regexp and antispam\_tag\_header\_ham\_regexp.

## Directories

#### `home`

List home

- Default:

    `$EXPLDIR`

- Overrides:

    None.

Base directory of list configurations.

#### `etc`

Directory for configuration files

- Default:

    `$SYSCONFDIR`

- Overrides:

    None.

Base directory of global configuration (except "sympa.conf").

#### `spool`

Base directory of spools

- Default:

    `$SPOOLDIR`

- Overrides:

    None.

Base directory of all spools which are created at runtime. This directory must be writable by Sympa user.

#### `queue`

Directory for message incoming spool

- Default:

    `$SPOOLDIR/msg`

- Overrides:

    None.

This spool is used both by "queue" program and "sympa\_msg.pl" daemon.

#### `queuemod`

Directory for moderation spool

- Default:

    `$SPOOLDIR/moderation`

- Overrides:

    None.

#### `queuedigest`

Directory for digest spool

- Default:

    `$SPOOLDIR/digest`

- Overrides:

    None.

#### `queueauth`

Directory for held message spool

- Default:

    `$SPOOLDIR/auth`

- Overrides:

    None.

This parameter is named such by historical reason.

#### `queueoutgoing`

Directory for archive spool

- Default:

    `$SPOOLDIR/outgoing`

- Overrides:

    None.

This parameter is named such by historical reason.

#### `queuesubscribe`

Directory for held request spool

- Default:

    `$SPOOLDIR/subscribe`

- Overrides:

    None.

This parameter is named such by historical reason.

#### `queuetopic`

Directory for topic spool

- Default:

    `$SPOOLDIR/topic`

- Overrides:

    None.

#### `queuebounce`

Directory for bounce incoming spool

- Default:

    `$SPOOLDIR/bounce`

- Overrides:

    None.

This spool is used both by "bouncequeue" program and "bounced.pl" daemon.

#### `queuetask`

Directory for task spool

- Default:

    `$SPOOLDIR/task`

- Overrides:

    None.

#### `queueautomatic`

Directory for automatic list creation spool

- Default:

    `$SPOOLDIR/automatic`

- Overrides:

    None.

This spool is used both by "familyqueue" program and "sympa\_automatic.pl" daemon.

#### `queuebulk`

Directory for message outgoing spool

- Default:

    `$SPOOLDIR/bulk`

- Overrides:

    None.

This parameter is named such by historical reason.

#### `tmpdir`

Temporary directory used by external programs such as virus scanner. Also, outputs to daemons' standard error are redirected to the files under this directory.

- Default:

    `$SPOOLDIR/tmp`

- Overrides:

    None.

#### `viewmail_dir`

Directory to cache formatted messages

- Default:

    `$SPOOLDIR/viewmail`

- Overrides:

    None.

Base directory path of directories where HTML view of messages are cached.

#### `bounce_path`

Directory for storing bounces

- Default:

    `$BOUNCEDIR`

- Overrides:

    None.

The directory where bounced.pl daemon will store the last bouncing message for each user. A message is stored in the file: &lt;bounce\_path>/&lt;list name>@&lt;mail domain name>/&lt;email address>, or, if tracking is enabled: &lt;bounce\_path>/&lt;list name>@&lt;mail domain name>/&lt;email address>\_&lt;envelope ID>.

Users can access to these messages using web interface in the bounce management page.

Don't confuse with "queuebounce" parameter which defines the spool where incoming error reports are stored and picked by bounced.pl daemon.

#### `arc_path`

Directory for storing archives

- Default:

    `$ARCDIR`

- Overrides:

    Virtual domain

Where to store HTML archives. This parameter is used by the "archived.pl" daemon. It is a good idea to install the archive outside the web document hierarchy to prevent overcoming of WWSympa's access control.

#### `purge_spools_task`

Task for cleaning spools

- Default:

    `daily`

- Overrides:

    None.

This task cleans old content in spools.

#### `clean_delay_queue`

Max age of incoming bad messages

- Default:

    `7` (days)

- Overrides:

    None.

Number of days "bad" messages are kept in message incoming spool (as specified by "queue" parameter). Sympa keeps messages rejected for various reasons (badly formatted, looping etc.).

#### `clean_delay_queueoutgoing`

Max age of bad messages for archives

- Default:

    `7` (days)

- Overrides:

    None.

Number of days "bad" messages are kept in message archive spool (as specified by "queueoutgoing" parameter). Sympa keeps messages rejected for various reasons (unable to create archive directory, to copy file etc.).

#### `clean_delay_queuebounce`

Max age of bad bounce messages

- Default:

    `7` (days)

- Overrides:

    None.

Number of days "bad" messages are kept in bounce spool (as specified by "queuebounce" parameter). Sympa keeps messages rejected for various reasons (unknown original sender, unknown report type).

#### `clean_delay_queuemod`

Max age of moderated messages

- Default:

    `30` (days)

- Overrides:

    List

Number of days messages are kept in moderation spool (as specified by "queuemod" parameter). Beyond this deadline, messages that have not been processed are deleted.

#### `clean_delay_queueauth`

Max age of held messages

- Default:

    `30` (days)

- Overrides:

    None.

Number of days messages are kept in held message spool (as specified by "queueauth" parameter). Beyond this deadline, messages that have not been confirmed are deleted.

#### `clean_delay_queuesubscribe`

Max age of held requests

- Default:

    `30` (days)

- Overrides:

    None.

Number of days requests are kept in held request spool (as specified by "queuesubscribe" parameter). Beyond this deadline, requests that have not been validated nor declined are deleted.

#### `clean_delay_queuetopic`

Max age of tagged topics

- Default:

    `30` (days)

- Overrides:

    None.

Number of days (automatically or manually) tagged topics are kept in topic spool (as specified by "queuetopic" parameter). Beyond this deadline, tagging is forgotten.

#### `clean_delay_queueautomatic`

Max age of incoming bad messages in automatic list creation spool

- Default:

    `10` (days)

- Overrides:

    None.

Number of days "bad" messages are kept in automatic list creation spool (as specified by "queueautomatic" parameter). Sympa keeps messages rejected for various reasons (badly formatted, looping etc.).

#### `clean_delay_queuebulk`

Max age of outgoing bad messages

- Default:

    `7` (days)

- Overrides:

    None.

Number of days "bad" messages are kept in message outgoing spool (as specified by "queuebulk" parameter). Sympa keeps messages rejected for various reasons (failed personalization, bad configuration on MTA etc.).

#### `clean_delay_queuedigest`

Max age of bad messages in digest spool

- Default:

    `14` (days)

- Overrides:

    None.

Number of days "bad" messages are kept in digest spool (as specified by "queuedigest" parameter). Sympa keeps messages rejected for various reasons (syntax errors in "digest.tt2" template etc.).

#### `clean_delay_tmpdir`

Max age of temporary files

- Default:

    `7` (days)

- Overrides:

    None.

Number of days files in temporary directory (as specified by "tmpdir" parameter), including standard error logs, are kept.

## Miscellaneous

#### `email`

Local part of Sympa email address

- Default:

    `sympa`

- Overrides:

    Virtual domain

Local part (the part preceding the "@" sign) of the address by which mail interface of Sympa accepts mail commands.

If you change the default value, you must modify the mail aliases too.

#### `listmaster_email`

Local part of listmaster email address

- Default:

    `listmaster`

- Overrides:

    Virtual domain

Local part (the part preceding the "@" sign) of the address by which listmasters receive messages.

If you change the default value, you must modify the mail aliases too.

#### `custom_robot_parameter`

Custom robot parameter

- Default:

    None.

- Overrides:

    Virtual domain

Used to define a custom parameter for your server. Do not forget the semicolon between the parameter name and the parameter value.

You will be able to access the custom parameter value in web templates by variable "conf.custom\_robot\_parameter.&lt;param\_name>"

Example:

    custom_robot_parameter param_name ; param_value

#### `cache_list_config`

Use of binary cache of list configuration

- Default:

    `none`

- Overrides:

    None.

binary\_file: Sympa processes will maintain a binary version of the list configuration, "config.bin" file on local disk. If you manage a big amount of lists (1000+), it should make the web interface startup faster.

You can recreate cache by running "sympa.pl --reload\_list\_config".

#### `db_list_cache`

Use database cache to search lists

- Default:

    `off`

- Overrides:

    None.

Note that "list\_table" database table should be filled at the first time by running:

    # sympa.pl --sync_list_db

#### `purge_user_table_task`

Task for expiring inactive users

- Default:

    `monthly`

- Overrides:

    None.

This task removes rows in the "user\_table" table which have not corresponding entries in the "subscriber\_table" table.

#### `purge_logs_table_task`

Task for cleaning tables

- Default:

    `daily`

- Overrides:

    None.

This task cleans old logs from "logs\_table" table.

#### `logs_expiration_period`

Max age of logs in database

- Default:

    `3` (months)

- Overrides:

    None.

Number of months that elapse before a log is expired

#### `stats_expiration_period`

Max age of statistics information in database

- Default:

    `3` (months)

- Overrides:

    None.

Number of months that elapse before statistics information are expired

#### `umask`

Umask

- Default:

    `027`

- Overrides:

    None.

Default mask for file creation (see umask(2)). Note that it will be interpreted as an octal value.

#### `cookie`

Secret string for generating unique keys

- Default:

    None.

- Overrides:

    List

This allows generated authentication keys to differ from a site to another. It is also used for encryption of user passwords stored in the database. The presence of this string is one reason why access to "sympa.conf" needs to be restricted to the "sympa" user.

Note that changing this parameter will break all HTTP cookies stored in users' browsers, as well as all user passwords and lists X509 private keys. To prevent a catastrophe, Sympa refuses to start if this "cookie" parameter was changed.

Example:

    cookie 123456789

## Web interface parameters

#### `wwsympa_url`

URL prefix of web interface

- Default:

    None.

- Overrides:

    Virtual domain

This is used to construct URLs of web interface.

Example:

    wwsympa_url https://web.example.org/sympa

#### `http_host`

URL prefix of WWSympa behind proxy

- Default:

    None.

- Overrides:

    Virtual domain

#### `static_content_url`

URL for static contents

- Default:

    `/static-sympa`

- Overrides:

    Virtual domain

HTTP server have to map it with "static\_content\_path" directory.

#### `static_content_path`

Directory for static contents

- Default:

    `$STATICDIR`

- Overrides:

    Virtual domain

#### `log_facility`

System log facility for web interface

- Default:

    `LOCAL1`

- Overrides:

    None.

System log facility for WWSympa, archived.pl and bounced.pl. Default is to use value of "syslog" parameter.

## Web interface parameters: Appearances

#### `logo_html_definition`

Custom logo

- Default:

    None.

- Overrides:

    Virtual domain

HTML fragment to insert a logo in the page of web interface.

Example:

    logo_html_definition <a href="http://www.example.com"><img style="float: left; margin-top: 7px; margin-left: 37px;" src="http://www.example.com/logos/mylogo.jpg" alt="My Company" /></a>

#### `favicon_url`

Custom favicon

- Default:

    None.

- Overrides:

    Virtual domain

URL of favicon image

#### `css_path`

Directory for static style sheets (CSS)

- Default:

    `$CSSDIR`

- Overrides:

    None.

After an upgrade, static CSS files are upgraded with the newly installed "css.tt2" template. Therefore, this is not a good place to store customized CSS files.

#### `css_url`

URL for style sheets (CSS)

- Default:

    `/static-sympa/css`

- Overrides:

    None.

To use auto-generated static CSS, HTTP server have to map it with "css\_path".

#### `pictures_path`

Directory for subscribers pictures

- Default:

    `$PICTURESDIR`

- Overrides:

    None.

#### `pictures_url`

URL for subscribers pictures

- Default:

    `/static-sympa/pictures`

- Overrides:

    None.

HTTP server have to map it with "pictures\_path" directory.

#### `color_0`, ..., `color_15`

Colors for web interface

- Default:

    See description on web interface.

- Overrides:

    Virtual domain

Colors are used in style sheet (CSS). They may be changed using web interface by listmasters.

#### `dark_color`, `light_color`, `text_color`, `bg_color`, `error_color`, `selected_color`, `shaded_color`

Colors for web interface, obsoleted

- Default:

    See description on web interface.

- Overrides:

    Virtual domain

#### `default_home`

Type of main web page

- Default:

    `home`

- Overrides:

    Virtual domain

"lists" for the page of list of lists. "home" for home page.

#### `archive_default_index`

Default index organization of web archive

- Default:

    `thrd`

- Overrides:

    None.

thrd: Threaded index.

mail: Chronological index.

#### `review_page_size`

Size of review page

- Default:

    `25`

- Overrides:

    Virtual domain

Default number of lines of the array displaying users in the review page

#### `viewlogs_page_size`

Size of viewlogs page

- Default:

    `25`

- Overrides:

    Virtual domain

Default number of lines of the array displaying the log entries in the logs page.

#### `main_menu_custom_button_1_title`, ... `main_menu_custom_button_3_title`, `main_menu_custom_button_1_url`, ... `main_menu_custom_button_3_url`, `main_menu_custom_button_1_target`, ... `main_menu_custom_button_3_target`

Custom menus

- Default:

    None.

- Overrides:

    Virtual domain

You may modify the main menu content by editing the menu.tt2 file, but you can also edit these parameters in order to add up to 3 buttons. Each button is defined by a title (the text in the button), an URL and, optionally, a target.

Example:

    main_menu_custom_button_1_title FAQ
    main_menu_custom_button_1_url http://www.renater.fr/faq/universalistes/index
    main_menu_custom_button_1_target Help

## Web interface parameters: Miscellaneous

#### `cookie_domain`

HTTP cookies validity domain

- Default:

    `localhost`

- Overrides:

    Virtual domain

If beginning with a dot ("."), the cookie is available within the specified Internet domain. Otherwise, for the specified host. The only reason for replacing the default value would be where WWSympa's authentication process is shared with an application running on another host.

Example:

    cookie_domain .renater.fr

#### `cookie_expire`

HTTP cookies lifetime

- Default:

    `0`

- Overrides:

    None.

This is the default value when not set explicitly by users. "0" means the cookie may be retained during browser sessions.

#### `cookie_refresh`

Average interval to refresh HTTP session ID.

- Default:

    `60`

- Overrides:

    None.

#### `purge_session_table_task`

Task for cleaning old sessions

- Default:

    `daily`

- Overrides:

    None.

This task removes old entries in the "session\_table" table.

#### `session_table_ttl`

Max age of sessions

- Default:

    `2d`

- Overrides:

    None.

Session duration is controlled by "sympa\_session" cookie validity attribute. However, by security reason, this delay also need to be controlled by server side. This task removes old entries in the "session\_table" table.

Format of values is a string without spaces including "y" for years, "m" for months, "d" for days, "h" for hours, "min" for minutes and "sec" for seconds.

#### `anonymous_session_table_ttl`

- Default:

    `1h`

- Overrides:

    None.

#### `shared_feature`

Enable shared repository

- Default:

    `off`

- Overrides:

    Virtual domain

If set to "on", list owners can open shared repository.

#### `default_shared_quota`

Default disk quota for shared repository

- Default:

    None (Kbytes).

- Overrides:

    Virtual domain

    List (`shared_doc.quota`)

#### `use_html_editor`

Use HTML editor

- Default:

    `0`

- Overrides:

    Virtual domain

If set to "on", users will be able to post messages in HTML using a javascript WYSIWYG editor.

Example:

    use_html_editor on

#### `html_editor_url`

URL of HTML editor

- Default:

    None.

- Overrides:

    Virtual domain

URL path to the javascript file making the WYSIWYG HTML editor available.  Relative path under &lt;static\_content\_url> or absolute path.

Example is for TinyMCE 4 installed under &lt;static\_content\_path>/js/tinymce/.

Example:

    html_editor_url js/tinymce/tinymce.min.js

#### `html_editor_init`

HTML editor initialization

- Default:

    None.

- Overrides:

    Virtual domain

Javascript excerpt that enables and configures the WYSIWYG HTML editor.

Example:

    html_editor_init tinymce.init({selector:"#body",language:lang.split(/[^a-zA-Z]+/).join("_")});

#### `max_wrong_password`

Count limit of wrong password submission

- Default:

    `19`

- Overrides:

    Virtual domain

If this limit is reached, the account is locked until the user renews their password. The default value is chosen in order to block bots trying to log in using brute force strategy. This value should never be reached by real users that will probably uses the renew password service before they performs so many tries.

#### `password_case`

Password case

- Default:

    `insensitive`

- Overrides:

    None.

"insensitive" or "sensitive".

If set to "insensitive", WWSympa's password check will be insensitive. This only concerns passwords stored in the Sympa database, not the ones in LDAP.

Should not be changed! May invalid all user password.

#### `password_hash`

Password hashing algorithm

- Default:

    `md5`

- Overrides:

    None.

"md5" or "bcrypt".

If set to "md5", Sympa will use MD5 password hashes. If set to "bcrypt", bcrypt hashes will be used instead. This only concerns passwords stored in the Sympa database, not the ones in LDAP.

Should not be changed! May invalid all user passwords.

#### `password_hash_update`

Update password hashing algorithm when users log in

- Default:

    `1`

- Overrides:

    None.

On successful login, update the encrypted user password to use the algorithm specified by "password\_hash". This allows for a graceful transition to a new password hash algorithm. A value of 0 disables updating of existing password hashes.  New and reset passwords will use the "password\_hash" setting in all cases.

#### `bcrypt_cost`

Bcrypt hash cost

- Default:

    `12`

- Overrides:

    None.

When "password\_hash" is set to "bcrypt", this sets the "cost" parameter of the bcrypt hash function. The default of 12 is expected to require approximately 250ms to calculate the password hash on a 3.2GHz CPU. This only concerns passwords stored in the Sympa database, not the ones in LDAP.

Can be changed but any new cost setting will only apply to new passwords.

#### `one_time_ticket_lifetime`

Age of one time ticket

- Default:

    `2d`

- Overrides:

    None.

Duration before the one time tickets are expired

#### `one_time_ticket_lockout`

Restrict access to one time ticket

- Default:

    `one_time`

- Overrides:

    Virtual domain

Is access to the one time ticket restricted, if any users previously accessed? (one\_time | remote\_addr | open)

#### `purge_one_time_ticket_table_task`

- Default:

    `daily`

- Overrides:

    None.

#### `one_time_ticket_table_ttl`

- Default:

    `10d`

- Overrides:

    None.

#### `pictures_feature`

Pictures

- Default:

    `on`

- Overrides:

    Virtual domain

    List

Enables or disables the pictures feature by default.  If enabled, subscribers can upload their picture (from the "Subscriber option" page) to use as an avatar.

Pictures are stored in a directory specified by the "static\_content\_path" parameter.

#### `pictures_max_size`

The maximum size of uploaded picture

- Default:

    `102400` (bytes)

- Overrides:

    Virtual domain

#### `spam_protection`

Protect web interface against spam harvesters

- Default:

    `javascript`

- Overrides:

    Virtual domain

These values are supported:

javascript: the address is hidden using a javascript. Users who enable Javascript can see nice mailto addresses where others have nothing.

at: the "@" character is replaced by the string "AT".

none: no protection against spam harvesters.

#### `web_archive_spam_protection`

Protect web archive against spam harvesters

- Default:

    `cookie`

- Overrides:

    Virtual domain

    List

The same as "spam\_protection", but restricted to the web archive.

In addition to it:

cookie: users must submit a small form in order to receive a cookie before browsing the web archive.

#### `reporting_spam_script_path`

Script to report spam

- Default:

    None.

- Overrides:

    Virtual domain

If set, when a list moderator report undetected spams for list moderation, this external script is invoked and the message is injected into standard input of the script.

#### `domains_blacklist`

Prevent people to subscribe to a list with adresses using these domains

- Default:

    None.

- Overrides:

    None.

This parameter is a comma-separated list.

Example:

    domains_blacklist example.org,spammer.com

#### `quiet_subscription`

Quiet subscriptions policy

- Default:

    `optional`

- Overrides:

    None.

Global policy for quiet subscriptions: "on" means that subscriptions will never send a notice to the subscriber, "off" will enforce a notice sending, and "optional" (default) allows the use of the list policy.

## S/MIME and TLS

S/MIME authentication, decryption and re-encryption. It requires these external modules: Crypt-OpenSSL-X509 and Crypt-SMIME.

TLS client authentication. It requires an external module: IO-Socket-SSL.

#### `cafile`

File containing trusted CA certificates

- Default:

    None.

- Overrides:

    None.

This can be used alternatively and/or additionally to "capath".

#### `capath`

Directory containing trusted CA certificates

- Default:

    None.

- Overrides:

    None.

CA certificates in this directory are used for client authentication.

The certificates need to have names including hash of subject, or symbolic links to them with such names. The links may be created by using "c\_rehash" script bundled in OpenSSL.

#### `key_passwd`

Password used to crypt lists private keys

- Default:

    None.

- Overrides:

    None.

If not defined, Sympa assumes that list private keys are not encrypted.

Example:

    key_passwd your_password

#### `ssl_cert_dir`

Directory containing user certificates

- Default:

    `$EXPLDIR/X509-user-certs`

- Overrides:

    None.

## Data sources setup

Including subscribers, owners and moderators from data sources. Appropriate database driver (DBD) modules are required: DBD-CSV, DBD-mysql, DBD-ODBC, DBD-Oracle, DBD-Pg, DBD-SQLite and/or Net-LDAP. And also, if secure connection (LDAPS) to LDAP server is required: IO-Socket-SSL.

#### `default_sql_fetch_timeout`

Default of SQL fetch timeout

- Default:

    `300`

- Overrides:

    List (`sql_fetch_timeout`)

Default timeout while performing a fetch with include\_sql\_query.

#### `default_ttl`

Default of inclusion timeout

- Default:

    `3600`

- Overrides:

    List (`ttl`)

Default timeout between two scheduled synchronizations of list members with data sources.

## DKIM and ARC

DKIM signature verification and re-signing. It requires an external module: Mail-DKIM.

ARC seals on forwarded messages. It requires an external module: Mail-DKIM.

#### `dkim_feature`

Enable DKIM

- Default:

    `off`

- Overrides:

    Virtual domain

    List

If set to "on", Sympa may verify DKIM signatures of incoming messages and/or insert DKIM signature to outgoing messages.

#### `dkim_add_signature_to`

Which service messages to be signed

- Default:

    `robot,list`

- Overrides:

    Virtual domain

Inserts a DKIM signature to service messages in context of robot, list or both

#### `dkim_private_key_path`

File path for DKIM private key

- Default:

    None.

- Overrides:

    Virtual domain

    List (`dkim_parameters.private_key_path`)

The file must contain a PEM encoded private key

#### `dkim_signature_apply_on`

Which messages delivered via lists to be signed

- Default:

    `md5_authenticated_messages,smime_authenticated_messages,dkim_authenticated_messages,editor_validated_messages`

- Overrides:

    Virtual domain

    List

Type of message that is added a DKIM signature before distribution to subscribers. Possible values are "none", "any" or a list of the following keywords: "md5\_authenticated\_messages", "smime\_authenticated\_messages", "dkim\_authenticated\_messages", "editor\_validated\_messages".

#### `dkim_signer_domain`

The "d=" tag as defined in rfc 4871

- Default:

    None.

- Overrides:

    Virtual domain

    List (`dkim_parameters.signer_domain`)

The DKIM "d=" tag is the domain of the signing entity. The virtual host domain name is used as its default value

#### `dkim_signer_identity`

The "i=" tag as defined in rfc 4871

- Default:

    None.

- Overrides:

    Virtual domain

Default is null.

#### `dkim_selector`

Selector for DNS lookup of DKIM public key

- Default:

    None.

- Overrides:

    Virtual domain

    List (`dkim_parameters.selector`)

The selector is used in order to build the DNS query for public key. It is up to you to choose the value you want but verify that you can query the public DKIM key for "&lt;selector>.\_domainkey.your\_domain"

#### `arc_feature`

Enable ARC

- Default:

    `off`

- Overrides:

    Virtual domain

    List

If set to "on", Sympa may add ARC seals to outgoing messages.

#### `arc_srvid`

SRV ID for Authentication-Results used in ARC seal

- Default:

    None.

- Overrides:

    Virtual domain

Typically the domain of the mail server

#### `arc_signer_domain`

The "d=" tag as defined in ARC

- Default:

    None.

- Overrides:

    Virtual domain

    List (`arc_parameters.arc_signer_domain`)

The ARC "d=" tag is the domain of the signing entity. The DKIM d= domain name is used as its default value

#### `arc_selector`

Selector for DNS lookup of ARC public key

- Default:

    None.

- Overrides:

    Virtual domain

    List (`arc_parameters.arc_selector`)

The selector is used in order to build the DNS query for public key. It is up to you to choose the value you want but verify that you can query the public DKIM key for "&lt;selector>.\_domainkey.your\_domain". Default is the same selector as for DKIM signatures

#### `arc_private_key_path`

File path for ARC private key

- Default:

    None.

- Overrides:

    Virtual domain

    List (`arc_parameters.arc_private_key_path`)

The file must contain a PEM encoded private key. Defaults to same file as DKIM private key

## DMARC protection

Processes originator addresses to avoid some domains' excessive DMARC protection. This feature requires an external module: Net-DNS.

#### `dmarc_protection_mode`

Test mode(s) for DMARC Protection

- Default:

    None.

- Overrides:

    Virtual domain

    List (`dmarc_protection.mode`)

Do not set unless you want to use DMARC protection.

This is a comma separated list of test modes; if multiple are selected then protection is activated if ANY match.  Do not use dmarc\_\* modes unless you have a local DNS cache as they do a DNS lookup for each received message.

Example:

    dmarc_protection_mode dmarc_reject,dkim_signature

#### `dmarc_protection_domain_regex`

Regular expression for domain name match

- Default:

    None.

- Overrides:

    Virtual domain

    List (`dmarc_protection.domain_regex`)

This is used for the "domain\_regex" protection mode.

#### `dmarc_protection_phrase`

New From name format

- Default:

    `name_via_list`

- Overrides:

    Virtual domain

    List (`dmarc_protection.phrase`)

This is the format to be used for the sender name part of the new From header field.

#### `dmarc_protection_other_email`

New From address

- Default:

    None.

- Overrides:

    Virtual domain

    List (`dmarc_protection.other_email`)

## List address verification

Checks if an alias with the same name as the list to be created already exists on the SMTP server. This feature requires an external module: Net-SMTP.

#### `list_check_helo`

SMTP HELO (EHLO) parameter used for address verification

- Default:

    None.

- Overrides:

    Virtual domain

Default value is the host part of "list\_check\_smtp" parameter.

#### `list_check_smtp`

SMTP server to verify existence of the same addresses as the list to be created

- Default:

    None.

- Overrides:

    Virtual domain

This is needed if you are running Sympa on a host but you handle all your mail on a separate mail relay.

Default value is real FQDN of the host. Port number may be specified as "mail.example.org:25" or "203.0.113.1:25".  If port is not specified, standard port (25) will be used.

#### `list_check_suffixes`

Address suffixes to verify

- Default:

    `request,owner,editor,unsubscribe,subscribe`

- Overrides:

    Virtual domain

List of suffixes you are using for list addresses, i.e. "mylist-request", "mylist-owner" and so on.

This parameter is used with the "list\_check\_smtp" parameter. It is also used to check list names at list creation time.

## Antivirus plug-in

#### `antivirus_path`

Path to the antivirus scanner engine

- Default:

    None.

- Overrides:

    Virtual domain

Supported antivirus: Clam AntiVirus/clamscan & clamdscan, McAfee/uvscan, Fsecure/fsav, Sophos, AVP and Trend Micro/VirusWall

Example:

    antivirus_path /usr/local/bin/clamscan

#### `antivirus_args`

Antivirus plugin command line arguments

- Default:

    None.

- Overrides:

    Virtual domain

Example:

    antivirus_args --no-summary --database /usr/local/share/clamav

#### `antivirus_notify`

Notify sender if virus checker detects malicious content

- Default:

    `sender`

- Overrides:

    Virtual domain

"sender" to notify originator of the message, "delivery\_status" to send delivery status, or "none"

## Password validation

Checks if the password the user submitted has sufficient strength. This feature requires an external module: Data-Password.

#### `password_validation`

Password validation

- Default:

    None.

- Overrides:

    None.

The password validation techniques to be used against user passwords that are added to mailing lists. Options come from Data::Password (http://search.cpan.org/~razinf/Data-Password-1.07/Password.pm#VARIABLES)

Example:

    password_validation MINLEN=8,GROUPS=3,DICTIONARY=4,DICTIONARIES=/pentest/dictionaries

## Authentication with LDAP

Authenticates users based on the directory on LDAP server. This feature requires an external module: Net-LDAP. And also, if secure connection (LDAPS) is required: IO-Socket-SSL.

#### `ldap_force_canonical_email`

Use canonical email address for LDAP authentication

- Default:

    `1`

- Overrides:

    Virtual domain

When using LDAP authentication, if the identifier provided by the user was a valid email, if this parameter is set to false, then the provided email will be used to authenticate the user. Otherwise, use of the first email returned by the LDAP server will be used.

## SOAP HTTP interface

Provides some functions of Sympa through the SOAP HTTP interface. This feature requires an external module: SOAP-Lite.

#### `soap_url`

URL of SympaSOAP

- Default:

    None.

- Overrides:

    Virtual domain

WSDL document of SympaSOAP refers to this URL in its service section.

Example:

    soap_url http://web.example.org/sympasoap

#### `soap_url_local`

URL of SympaSOAP behind proxy

- Default:

    None.

- Overrides:

    Virtual domain

## Obsoleted parameters

#### `host`

- Default:

    None.

- Overrides:

    Virtual domain

#### `log_condition`

- Default:

    None.

- Overrides:

    Virtual domain

#### `log_module`

- Default:

    None.

- Overrides:

    Virtual domain

#### `filesystem_encoding`

- Default:

    `utf-8`

- Overrides:

    None.

#### `automatic_list_prefix`

Defines the prefix allowing to recognize that a list is an automatic list.

- Default:

    None.

- Overrides:

    None.

#### `default_distribution_ttl`

Default timeout between two action-triggered synchronizations of list members with data sources.

- Default:

    `300`

- Overrides:

    None.

#### `edit_list`

- Default:

    `owner`

- Overrides:

    None.

#### `use_fast_cgi`

Enable FastCGI

- Default:

    `1`

- Overrides:

    None.

Is FastCGI module for HTTP server installed? This module provides a much faster web interface.

#### `htmlarea_url`

- Default:

    None.

- Overrides:

    None.

#### `show_report_abuse`

Add a "Report abuse" link in the side menu of the lists (0|1)

- Default:

    `0`

- Overrides:

    None.

The link is a mailto link, you can change that by overriding web\_tt2/report\_abuse.tt2

#### `allow_account_deletion`

EXPERIMENTAL! Allow users to delete their account. If enabled, shows a "delete my account" form in user's preferences page.

- Default:

    `0`

- Overrides:

    None.

Account deletion unsubscribes the users from his/her lists and removes him/her from lists ownership. It is only available to users using internal authentication (i.e. no LDAP, no SSO...). See https://github.com/sympa-community/sympa/issues/300 for details

# FILES

- `/etc/sympa/sympa.conf`

    Sympa main configuration file.

- `$SYSCONFDIR/<robot name>/robot.conf`

    Configuration specific to each virtual domain.

- `$EXPLDIR/<list name>/config`
or `$EXPLDIR/<robot name>/<list name>/config`

    Configuration specific to each list.

# SEE ALSO

_Sympa Administration Manual_.
[https://sympa-community.github.io/manual/](https://sympa-community.github.io/manual/).
