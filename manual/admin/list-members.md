---
title: 'Managing list members, owners and moderators'
prev: list-creation.md
up: ../admin.md
---

Managing list members, owners and moderators
============================================

The standard way to define list members is to make them subscribe to the list (or be added by the list owner). However, you may have users groups already defined in your LDAP directory or in a relational database. Therefore Sympa allows you to automatically map list membership to such data sources. The term **include** is used to designate these dynamic mailing list membership managements.

Standard definition of list members and owners
----------------------------------------------

We'll first describe the standard way for Sympa to handle list members and owners definition.

### Subscribed members

List members are often named *subscribers* because the default way to become member of a list is to subscribe to it. The subscription action can be performed via any of the three Sympa user interface : mail, web or SOAP. The mail command for subscribing is *SUBSCRIBE,* see ~~[user_commands](../mail-commands.md#user_commands)~~. Subscription feature is available from the list homepage on the web interface, but it requires prior user authentication.

The configuration of a list may restrict the right to subscribe to this list (closed, moderated, restricted to certain user categories,...) ; this is configured via [the subscribe list parameter](/gpldoc/man/sympa_config.5.html#subscribe). [A similar list configuration parameter](/gpldoc/man/sympa_config.5.html#unsubscribe) defines if list members can unsubscribe from the list. If the subscription process is moderated, then list owners will receive mail notifications for each subscription request; they can then validate the request via either web or mail interfaces of Sympa.

List owners can also add list members, though this feature should be used with care since users can easily confuse this with spam. It is easier for a subscriber to unsubscribe if (s)he did subscribe himself. List owners can use the ~~[ADD](../mail-commands.md#owner_commands)~~ mail command or the web interface to add new list members. The default right to add and remove list members is provided to list owners, but this behavior can be customized through the [add](/gpldoc/man/sympa_config.5.html#add) and [del](/gpldoc/man/sympa_config.5.html#del) list configuration parameters.

Sympa stores the list membership data in its database, in the [`subscriber_table` table](/gpldoc/man/sympa_database.5.html#subscriber_table). Along with the list name and member email addresses, Sympa stores the following informations: user name, date of subscription, date of latest update, visibility option, reception option, selected topics, bouncing informations.

### Declaring list owners or moderators

A list owner is assigned to a list at the list creation time ; it corresponds to the user who requests the list creation. Additional list owners can later be addded by existing list owners or by the listmaster; this is achieved through the list admin web interface. List owners are defined by the [owner list configuration parameter](/gpldoc/man/sympa_config.5.html#owner).

List moderators are managed the same way, defined by the [`editor` list configuration parameter](/gpldoc/man/sympa_config.5.html#editor). Note that if no list moderator is defined, then the list owners are taken as defaults for them.

Sympa defines two profiles for list owners: normal and privileged. Privileged owners have extended privileges, including the right to edit some list configuration parameters; this can be customized through the [`edit_list.conf`](/gpldoc/man/edit_list.conf.5.html) configuration file.
See also "[List editing](list-creation.md#list-editing)".

List owners and moderators are defined in the list configuration file, however, for performances reasons, list owners are also listed in the [`admin_table` DB table](/gpldoc/man/sympa_database.5.html#admin_table). This cache is updated by Sympa processes whenever the list configuration has changed on disk.

Importing subscribers from text file
------------------------------------

You can import subscribers from text file.  See `--import` option in [sympa(1)](/gpldoc/man/symap.1.html) for details.

Dynamic mailing lists
---------------------

Sympa provides a way to have list membership based on third-party data sources. These data sources can be any of the following :

  - another mailing list (local or remote)

  - a flat file (local or remote)

  - an SQL query

  - an LDAP search operation

Once the data sources defined, the Sympa server will cope with the data updates.

See "[Data sources](../customize/data-sources.md) for details.

