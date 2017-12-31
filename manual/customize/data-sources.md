Data sources
============

(Work in progress)

Data inclusion file
-------------------

Every file has the `.incl` extension. Moreover, these files must be declared in paragraphs `member_include`, `owner_include` or `editor_include` in the list configuration file (without the `.incl` extension) (see [List configuration parameters](../man/list_config.5.md#data-sources-setup)). This files can be template files.

Sympa looks for them in the following order:

  - [``$EXPLDIR``](../layout.md#expldir)`/mylist/data_sources/`*file*`.incl`;
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/data_sources/`*file*`.incl`;
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`*mail domain*`/data_sources/`*file*`.incl`.

These files are used by Sympa to load administrative data in a relational database: owners or editors are defined *intensively* (definition of criteria owners or editors must satisfy). Includes can be performed by extracting email addresses using an SQL or LDAP query, or by including other mailing lists.

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

Using Active Directory for Sympa data sources
---------------------------------------------

Active Directory having quite a specific functionning, *Steve Shipway* found a way to make it work with Sympa. Here is his guidelines to achieve this goal.

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

Now, how about an Exchange distribution list? You can always just chain to one, but why not use the distribution list as an external datasource so that the members can be properly loaded in?

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

Both of these `.incl` templates work for us on our Windows2013 system, and are in active use for list configurations.
