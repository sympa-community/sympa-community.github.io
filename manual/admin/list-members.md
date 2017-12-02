Managing list members, owners and editors
=========================================

The standard way to define list members is to make them subscribe to the list (or be added by the list owner). However, you may have users groups already defined in your LDAP directory or in a relational database. Therefore Sympa allows you to automatically map list membership to such data sources. The term **include** is used to designate these dynamic mailing list membership managements.

Standard definition of list members and owners
----------------------------------------------

We'll first describe the standard way for Sympa to handle list members and owners definition.

### Subscribed members

List members are often named *subscribers* because the default way to become member of a list is to subscribe to it. The subscription action can be performed via any of the three Sympa user interface : mail, web or SOAP. The mail command for subscribing is *SUBSCRIBE,* see ~~[user_commands](/manual/sympa-commands#user_commands)~~. Subscription feature is available from the list homepage on the web interface, but it requires prior user authentication.

The configuration of a list may restrict the right to subscribe to this list (closed, moderated, restricted to certain user categories,...) ; this is configured via [the subscribe list parameter](../man/list_config.5.md#subscribe). [A similar list configuration parameter](../man/list_config.5.md#unsubscribe) defines if list members can unsubscribe from the list. If the subscription process is moderated, then list owners will receive mail notifications for each subscription request; they can then validate the request via either web or mail interfaces of Sympa.

List owners can also add list members, though this feature should be used with care since users can easily confuse this with spam. It is easier for a subscriber to unsubscribe if (s)he did subscribe himself. List owners can use the ~~[ADD](/manual/sympa-commands#owner_commands)~~ mail command or the web interface to add new list members. The default right to add and remove list members is provided to list owners, but this behavior can be customized through the [add](../man/list_config.5.md#add) and [del](../man/list_config.5.md#del) list configuration parameters.

Sympa stores the list membership data in its database, in the [`subscriber_table` table](../man/sympa_database.5.md#subscriber_table). Along with the list name and member email addresses, Sympa stores the following informations: user name, date of subscription, date of latest update, visibility option, reception option, selected topics, bouncing informations.

### Declaring list owners or moderators

A list owner is assigned to a list at the list creation time ; it corresponds to the user who requests the list creation. Additional list owners can later be addded by existing list owners or by the listmaster; this is achieved through the list admin web interface. List owners are defined by the [owner list configuration parameter](../man/list_config.5.md#owner).

List moderators are managed the same way, defined by the [editor list configuration parameter](../man/list_config.5.md#editor). Note that if no list moderator is defined, then the list owners are taken as defaults for them.

Sympa defines two profiles for list owners: normal and privileged. Privileged owners have extended privileges, including the right to edit some list configuration parameters; this can be customized through the ~~[edit_list.conf configuration file](list-creation.md#list_editing)~~.

List owners and moderators are defined in the list configuration file, however, for performances reasons, list owners are also listed in the [`admin_table` DB table](../man/sympa_database.5.md#admin_table). This cache is updated by Sympa processes whenever the list configuration has changed on disk.

Dynamic mailing lists
---------------------

Sympa provides a way to have list membership based on third-party data sources. These data sources can be any of the following :

  - another mailing list (local or remote)

  - a flat file (local or remote)

  - an SQL query

  - an LDAP directory filter

Once the data sources defined, the Sympa server will cope with the data updates.

### Defining the data sources for members

The supported data sources should all return a set of email addresses, because that's the information Sympa needs to define a list member. You can define as many data sources as required, including data sources of the same type.

Data sources are defined through [list-xx configuration parameters](../man/list_config.5.md#data-sources-setup); they can be edited through the list admin web interface of Sympa. Note that the ~~[user_data_source list configuration parameter](../man/list_config.5.md#user_data_source)~~ is no more used (hard-coded include2 value) ; it has been introduced to support different members data management modes.

### Defining the data sources for list owners and moderators

List owners can be defined using external data sources the same way members are. The main difference is related to the configuration parameters : data sources are not directly defined in the list configuration file, they come from a separate `.inc` file located in the ~~[data_sources/ directory](/manual/list-definition#data_inclusion_file)~~. The [owner_include list configuration parameter](../man/list_config.5.md#owner_include) then refers to this data source file. This different configuration approach has been adopted to lessen the number of list configuration parameters.

List moderators are defined the same way with the [editor_include configuration parameter](../man/list_config.5.md#editor_include).

### Cache management

Sympa maintains a cache of included list members in the `subscriber_table` DB table. The update of the cache is mainly performed by the [task_manager.pl](../man/task_manager.1.md) process (via the `sync_include` task) ; the frequency of the updates is defined by the [`ttl` list configuration parameter](../man/list_config.5.md#ttl). However, an update can be performed by other processes under the following circumstances:

  - [wwsympa.fcgi](../man/wwsympa.8.md), using the "Synchronize members with data source" button, from the members review page;

  - wwsympa.fcgi, after a data-source related list configuration parameter has been edited;

  - [sympa.pl](../man/sympa.1.md) in command line, using the `--sync_include` option;

  - sympa.pl, before sending a message for a list (depends on the [`distribution_ttl` parameter](../man/list_config.5.md#distribution_ttl));

  - sympa.pl or wwsympa.fcgi or [sympa_soap_server.fcgi](../man/sympa_soap_server.8.md), while performing a members review (depends on the [`distribution_ttl` parameter](../man/list_config.5.md#distribution_ttl)).

If one or more data source is unreachable, the latest data will stay in the cache.

Sympa stores 3 kinds of include-related informations in the list members DB row :

  - `subscribed_subscriber`: set to 1 if the member did subscribe (or was added by the list owner) to the list

  - `included_subscriber`: set to 1 if the member was included from an external data source(s)

  - `include_sources_subscriber`: a comma-separated list of external source identiers. These identifiers are computed by Sympa processes.


