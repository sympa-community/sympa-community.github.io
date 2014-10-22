Data sources
============

(Work in progress)


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
