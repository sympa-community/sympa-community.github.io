---
title: 'Roles and privileges'
prev: basics-workflow.md
up: ../customize.md#customization-basics
next: basics-configuration.md
---

Roles and privileges
====================

You can assign roles to users (identified by their email addresses) at different levels in Sympa; privileges are associated (or can be associated) to these roles. We list these roles below (from the most powerful to the least), along with the relevant privileges.

(Super) listmasters
-------------------

These are the persons administrating the service, defined in the [`sympa.conf`](/gpldoc/man/sympa_config.5.html) file. They inherit the listmaster role in virtual hosts and are the default set of listmasters for virtual hosts.

(Robot) listmasters
-------------------

You can define a different set of listmasters at a virtual host level (in the [`robot.conf`](/gpldoc/man/sympa_config.5.html) file). They are responsible for moderating mailing lists creation (if list creation is configured this way), editing default templates, providing help to list owners and moderators. Users defined as listmasters get a privileged access to the Sympa web interface. Listmasters also inherit the privileges of list owners (for any list defined in the virtual host), but not the moderator privileges.

Privileged list owners
----------------------

The first defined privileged owner is the person who requested the list creation. Later it can be changed or extended. They inherit (basic) owner privileges and are also responsible for managing the list owners and moderators themselves (through the web interface). With Sympa's default behavior, privileged owners can edit more list parameters than (basic) owners can do; but this can be customized via the [`edit_list.conf`](/gpldoc/man/edit_list.conf.5.html) file.

(Basic) list owners
-------------------

They are responsible for managing the members of the list, editing the list configuration and templates. Owners (and privileged owners) are defined in the list [`config`](/gpldoc/man/sympa_config.5.html) file.

Moderators (also called "editors")
----------------------------------

Moderators are responsible for the messages distributed in the mailing list (as opposed to owners who look after list members). Moderators are active if the list has been setup as a moderated mailing list. If no moderator is defined for the list, then list owners will inherit the moderator role.

Subscribers (or "list members")
-------------------------------

Subscribers are the people who are members of a mailing list; they either subscribed, or got added directly by the listmaster or via a data source (LDAP, SQL, another list, ...). These subscribers receive messages posted in the list (unless they have set the `nomail` option or suspended reception) and have special privileges to post in the mailing list (unless it is a newsletter list). Most privileges a subscriber may have are not hard coded in Sympa but expressed via the so-called [authorization scenarios](basics-scenarios.md).

