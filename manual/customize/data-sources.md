---
title: 'Data sources'
up: ../customize.md#sympa-services-optional-features
---

Data sources
============

Sympa can include list subscribers, owners and editors from external data
sources.
The supported data sources should all return a set of email addresses, because
that's the information Sympa needs to define a list user. You can define as
many data sources as required, including data sources of the same type.

Currently, these types of data sources are supported:

  - `include_file`

    Inclusion from the file on local server.

  - `include_remote_file`

    Inclusion from the file on remote server using HTTPS.

  - `include_sympa_list`

    Inclusion from the list on local server (either the same mail domain or
    not).

  - `include_remote_sympa_list`

    Inclusion from the list on remote Sympa server.

  - `include_ldap_query`

    Inclusion using LDAP search operation.

  - `include_ldap_2level_query`

    Inclusion using LDAP search operation twice.

  - `include_sql_query`

    Inclusion using SQL query to SQL server or local CSV file.

Requirements
------------

  - To use datasources for remote file or remote Sympa list,
    [IO-Socket-SSL](https://metacpan.org/release/IO-Socket-SSL) Perl module
    have to be installed.

  - To use datasources based on SQL query, appropriate DBI driver (DBD)
    Perl module corresponding to the database systems have to be installed: 
    [DBD-CSV](https://metacpan.org/release/DBD-CSV),
    [DBD-mysql](https://metacpan.org/release/DBD-mysql),
    [DBD-ODBC](https://metacpan.org/release/DBD-ODBC),
    [DBD-Oracle](https://metacpan.org/release/DBD-Oracle),
    [DBD-Pg](https://metacpan.org/release/DBD-Pg),
    [DBD-SQLite](https://metacpan.org/release/DBD-SQLite) or
    [DBD-Sybase](https://metacpan.org/release/DBD-Sybase).

  - To use datasources based on LDAP search operation,
    [Net-LDAP](https://metacpan.org/release/Net-LDAP) Perl module have to be
    installed.  Additionally, if TLS connection to LDAP server --- LDAPS
    (LDAP over TLS) or Start_TLS extension --- should be supported,
    [IO-Socket-SSL](https://metacpan.org/release/IO-Socket-SSL) Perl module
    also have to be installed.

Defining the data sources
-------------------------

### List configuration parameters

Data source definition comes from a separate `.inc` file located in the
`data_sources/` directory (See "[Data inclusion file](#data-inclusion-file)").
Any of following list configuration parameters then refers to this data source
file.
  - [`member_include`](../man/list_config.5.md#member_include) for members.
  - [`owner_include`](../man/list_config.5.md#owner_include) for owners.
  - [`editor_include`](../man/list_config.5.md#editor_include) for moderators.

This configuration approach has been adopted to lessen the number of list
configuration parameters.

There is another way to define member data sources: Member data source may
also be defined using
[`include_*`](../man/list_config.5.md#data-sources-setup) configuration
parameters; they can be edited through the list admin web interface of Sympa.

----
Note:

  * As of Sympa 6.2,
    [`user_data_source`](../man/list_config.5.md#user_data_source)
    list configuration parameter is no more used (hard-coded include2 value);
    it has been introduced to support different members data management modes.

----

### Data inclusion file

Every file has the `.incl` extension. Moreover, these files must be declared in paragraphs `member_include`, `owner_include` or `editor_include` in the list configuration file (without the `.incl` extension) (see [list configuration parameters](../man/list_config.5.md#data-sources-setup)).  These files can be processed as template files.

Sympa looks for them in the following order:

  - [``$EXPLDIR``](../layout.md#expldir)`/`*list path*`/data_sources/`*file*`.incl`;
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`*mail domain*`/data_sources/`*file*`.incl`.
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/data_sources/`*file*`.incl`;

These files are used by Sympa to load administrative data in a relational database: owners or editors are defined *intensively* (definition of criteria owners or editors must satisfy). Includes can be performed by extracting email addresses using an SQL query or LDAP search operation, or by including other mailing lists.

A data inclusion file is made of paragraphs separated by blank lines and introduced by a keyword. Valid paragraphs are `include_file`, `include_remote_file`, `include_list`, `include_remote_sympa_list`, `include_sql_query`, `include_ldap_query` and `include_ldap_2level_query`. They are described in the [List configuration parameters](../man/list_config.5.md#data-sources-setup) chapter.

When this file is a template, the variables used are array elements (`param` array). This array is instantiated by values contained in the subparameter `source_parameter` of `owner_include` or `editor_include`.

Example:

  - in the list configuration file [``$EXPLDIR``](../layout.md#expldir)`/mylist/config` :

    ``` code
    owner_include
    source myfile
    source_parameters mysql,rennes1,stduser,mysecret,studentbody,student
    ```

  - in [``$SYSCONFDIR``](../layout.md#sysconfdir)`/data_sources/myfile.incl`:

    ``` code
    include_sql_query
    db_type [% param.0 %]
    host sqlserv.admin.univ-[% param.1 %].fr
    user [% param.2 %]
    passwd [% param.3 %]
    db_name [% param.4 %]
    sql_query SELECT DISTINCT email FROM [% param.5 %]
    ```

  - once it has been parsed with provided parameters, the inclusion directives would look like this:

    ``` code
    include_sql_query
    db_type mysql
    host sqlserv.admin.univ-rennes1.fr
    user stduser
    passwd mysecret
    db_name studentbody
    sql_query SELECT DISTINCT email FROM student
    ```

Excluding members from dynamic update
-------------------------------------

Particular users can be excluded from dynamic update.
Excluded users will no longer be added nor deleted from the list when data
sources are updated.  Thus, owners may be able to add or delete those users
manually, and those users may be able to subscribe or unsubscribe themselves
(if they are allowed).

Information of excluded users are stored in
[`exclusion_table`](../man/sympa_database.5.md#exclusion_table) table of
database.  List owners can update the information in one of following ways:

  - Using "Manage list members" -- "Exclude" in list administration menu,
    list owners can add or remove users to be excluded.

  - Deleting a member included from data sources from the list, they are
    excluded automatically.

Note that exclusion will not be released even if the same user will be added
again.

----
Note:

  * On Sympa prior to 6.2, deleting member needed exclusion in advance, if
    they had already been included from data sources.

    On 6.2 or later, deleting may imply exclusion.  If you noticed that
    one or more users were not included from data sources, please check
    "Exclude" in list administration menu.

----

Cache management
----------------

Sympa maintains a cache of included list members in the [`subscriber_table`](../man/sympa_database.5.md#subscriber_table) database table. The update of the cache is mainly performed by the [task_manager.pl](../man/task_manager.8.md) process (via the `sync_include` task) ; the frequency of the updates is defined by the [`ttl`](../man/list_config.5.md#ttl) list configuration parameter. However, an update can be performed by other processes under the following circumstances:

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

Tip: Using Active Directory for Sympa data sources
--------------------------------------------------

Active Directory having quite a specific functionality, *Steve Shipway* found a way to make it work with Sympa. Here is his guidelines to achieve this goal.

First, create a service account in your LDAP so that Sympa can connect. This only needs read-only access.

You need to use a 2level query, since AD stores DNs against group membership. Also, note that if using a `.incl` file to define external list admins, you cannot pass a full DN as a parameter as it contains commas (I’ve logged a bug report for this).

In this example, replace `[% param.0 %]` with the group name (the `cn`, not the `dn`). Obviously, change the `host`, `user` and `passwd` to the appropriate values for your site, and also the `suffix1` to match your tree base.

``` code
include_ldap_2level_query
name  ad_group_[% param.0 %]
host  uoa.auckland.ac.nz
port  3269
user  uoasvcsympa
passwd XXXXXXXXX
use_ssl yes
ssl_version tls
suffix1 DC=UoA,DC=auckland,DC=ac,DC=nz
filter1 (&(cn=[% param.0 %])(objectClass=group))
attrs1  member
select1 all
timeout1 60
scope1 sub
suffix2 [attrs1]
filter2 (objectClass=person)
attrs2 mail
select2 first
scope2 base
timeout2 10
```

Note that we’re forcing TLS (since Poodle should have had you disabling SSLv2/3 on your AD LDAP!) and have a 60s timeout, which might be too low for huge groups with a slow AD. We have patched our Sympa to also retrieve `displayName` on the final lookup to populate the gecos data; however with vanilla Sympa you don’t get this.

Now, how about an Exchange distribution list? You can always just chain to one, but why not use the distribution list as an external data source so that the members can be properly loaded in?

In this example, `[% param.0 %]` is the full email address of the distribution list, such as `distlist@uoa.auckland.ac.nz`. Again, we use a 2level lookup, since a distribution list holds the DN of the members rather than their email addresses. `filter1` is possibly overkill, but we want to cover all different possible configurations of AD (you might be able to simplify it for your system).

``` code
include_ldap_2level_query
name exchange_[% param.0 %]
host uoa.auckland.ac.nz
port 3269
user uoasvcsympa
passwd XXXXXXX
use_ssl yes
ssl_version tls
suffix1 DC=UoA,DC=auckland,DC=ac,DC=nz
filter1 (|(proxyAddresses=[% param.0 %])(proxyAddresses=smtp:[% param.0 %])(mailLocalAddress=[% param.0 %])(mail=[% param.0 %]))
attrs1 member
select1 all
timeout1 60
scope1 sub
suffix2 [attrs1]
filter2 (objectClass=person)
attrs2 mail
select2 first
scope2 base
timeout2 10
```

Both of these `.incl` templates work for us on our Windows 2013 system, and are in active use for list configurations.

