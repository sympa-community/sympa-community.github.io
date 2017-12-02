---
title: Sponsoring plugin
up: template-plugins.md
---

Sponsoring plugin
=================

Sponsoring: plugin to allow subscription to list A (called sponsored list) by subscribers of list B (called sponsor list).

Install instructions
--------------------

### To enable the functionnality

  - untar [sponsoring.tgz](https://www.sympa.org/_media/templates_plugins/sponsoring.tgz) to the [``$MODULEDIR``](../layout.md#moduledir)`/Sympa/Template/Plugin` directory

  - copy the files from the mail\_tt2 sub-directory to the relevant mail\_tt2 directory (either in etc, etc/\[vhost\] or in list directory).

  - copy the content of custom\_actions subdirectory in the custom\_actions subdirectory of each list that will be sponsored.

  - copy Sponsoring/Commons.conf.example into Sponsoring/Commons.conf and edit this file. It contains a set of “godfathered\_list paragraphs. Below is a description of the godfathred\_list sub-parameters:

      - listname (mandatory): local part of the list name

      - host (mandatory): name of the vhost under which the list is hosted.

      - expiration\_period (mandatory): number of days to wait between the suppression of a sponsor and the unsubscription of her godsons.

      - sponsor\_lists (mandatory): a comma-separated list of the email addresses of the lists whose subscribers are allowed to subscribe people to the sponsored list.

      - max\_godson\_nb (mandatory): maximum number of godson a sponsor can have in the sponored list.

      - exclusive\_sponsoring (optional): if set to 1, godsons will be removed from the list of godsons by the check\_godsons.pl script if they becom subscribers of one of the sponsor lists.

  - create a read only user in the Sympa database.

### For each sponsored list

  - create a datasource paragraph in the config file:

``` code
include_sql_query
db_name [db name]
db_type [your db type]
user [sympa read only user name]
passwd [sympa read only password]
name Sponsoring
sql_query select plugin_godsons_godson from plugin_godsons where plugin_godsons_list = '[the list name]'
host [your db host name]
```

  - Add a paragraph in Sponsoring/Commons.pm to describe the sponsoring.

  - create web\_tt2/additional\_list\_menu\_links.tt2. It must contain the following:

``` code
<!-- start additionnal_list_menu_links.tt2 -->

[% IF action == 'lca' %][% SET class = 'active' %][% ELSE %][% SET class = '' %][% END %]
<li class="[% class %]"><a href="[% path_cgi %]/lca/sponsoring/[% list %]" >[%|loc%]Sponsoring[%END%]</a></li>

<!-- end additionnal_list_menu_links.tt2 -->
```

All set!
