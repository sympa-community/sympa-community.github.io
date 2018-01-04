---
title: 'List creation, editing and removal'
prev: cli.md
up: ../admin.md
next: list-members.md
---

List creation, editing and removal
==================================

The list creation can be done in two ways, according to listmaster needs:

  - family instanciation, to create and manage a large number of related lists. In this case, lists are linked to their family all along their life (moreover, you can let Sympa automatically create lists when needed. See "~~[Automatic lists](../customize/automatic-lists.md)~~").
  - command line creation of individual list with `sympa.pl` or on the web interface according to privileges defined by listmasters. In this case, lists are free from their creation model.

Management of mailing lists by list owners is usually done through the web interface: when a list is created, whatever its status (`pending` or `open`), the owners can use *WWSympa* administration features to modify list parameters, to edit the welcome message, and so on.

*WWSympa* keeps logs of the creation and all modifications to a list as part of the list's `config` file (old configuration files are archived). A complete installation requires some careful planning, although default values should be acceptable for most sites.

List creation
-------------

(Work in progress)

### Data for list creation

To create a list, some data concerning list parameters are required:

  - **listname**: name of the list;
  - **subject**: subject of the list (a short description);
  - **owner(s)**: by static definition and/or dynamic definition. In case of static definition, the parameter `owner` and its subparameter `email` are required. For dynamic definition, the parameter `owner_include` and its subparameter `source` are required, indicating source file of data inclusion;
  - **list creation template**: the typical list profile.

In addition to these required data, provided values are assigned to vars being in the list creation template. Then the result is the list configuration file:

On the web interface, these data are given by the list creator in the web form. On command line, these data are given through an XML file.

### Typical list profile

List profiles (list creation templates) are stored in [``$SYSCONFDIR``](../layout.md#sysconfdir)`/create_list_templates` or in [``$DEFAULTDIR``](../layout.md#defaultdir)`/create_list_templates` (default of distribution) as directories with profile name. Directory of a profile should contain at least two files: config.tt2 and comment.tt2.

comment.tt2 is the descripion of list profile. It may contain tilte line(s) and HTML content. They are shown in list creation page of web interface.

config.tt2 is the template of list configuration. It will be formatted and save as config file when list is created.

You might want to hide or modify profiles (not useful, or dangerous for your site). If a profile exists both in the local site directory [``$SYSCONFDIR``](../layout.md#sysconfdir)`/create_list_templates` and in the [``$DEFAULTDIR``](../layout.md#defaultdir)`/create_list_templates` directory, then the local profile will be used by *WWSympa*.

### comment.tt2

Title line(s) are placed at beginning of config.tt2. They are separated from content by single newline. Below is an example.

``` code
title Liste de discussion publique
title.fr Liste de discussion publique
title.gettext Public discussion mailing list

<ul><li>[%|loc%]public archives[%END%]</li>
<li>[%|loc%]only subscribers can post[%END%]</li>
</ul>
```

----
Note:

  * Title lines are available as of Sympa 6.2.

----

### XML file format

The XML file provides information on:

  - the list name;
  - values to assign vars in the list creation template;
  - the list description in order to be written in the list file information;
  - the name of the list creation template (only for list creation on command line with `sympa.pl`; in a family context, the template is specified by the family name).

Here is an example of XML document that you can map with the following example of list creation template:

``` xml
<?xml version="1.0" ?>
<list>
    <listname>example</listname>
    <type>my_profile</type>
    <subject>a list example</subject>
    <description/>
    <status>open</status>
    <shared_edit>editor</shared_edit>
    <shared_read>private</shared_read>
    <language>fr</language>
    <owner multiple="1">
        <email>serge.aumont@renater.fr</email>
        <gecos>C.R.U.</gecos>
    </owner>
    <owner multiple="1">
        <email>olivier.salaun@renater.fr</email>
    </owner>
    <owner_include multiple="1">
        <source>my_file</source>
    </owner_include>
    <moderator>
        <email>user@domain.org</email>
    </moderator>
    <topic>Computing</topic>
    <sql>
        <type>Oracle</type>
        <host>sqlserv.admin.univ-x.fr</host>
        <port>1521</port>
        <user>stdutilisateur</user>
        <pwd>monsecret</pwd>
        <name>les_etudiants</name>
        <env>ORACLE_HOME=/[oracle_path]</env>
        <query>SELECT DISTINCT email FROM etudiant</query>
    </sql>
</list>
```

Here is an example of list creation template, [``$DEFAULTDIR``](../layout.md#defaultdir)`/create_list_templates/discussion_list/config.tt2`:

``` code
subject [% subject %]

status [% status %]

[% IF topic %]
topics [% topic %]

[% END %]
visibility noconceal

send privateoreditorkey

web_archive
  access public

subscribe open_notify

shared_doc
  d_edit [% shared_edit %]
  d_read [% shared_read %]

lang [% language %]

[% FOREACH o = owner %]
  owner
    email [% o.email %]
    profile privileged
    [% IF o.gecos %]
    gecos [% o.gecos %]
    [% END %]

[% END %]
[% IF moderator %]
  [% FOREACH m = moderator %]
    editor
      email [% m.email %]

  [% END %]
[% END %]

[% IF sql %]
  include_sql_query
    db_type [% sql.type %]
    db_port [% sql.port %]
    host [% sql.host %]
    user [% sql.user %]
    passwd [% sql.pwd %]
    db_name [% sql.name %]
    db_env [% sql.env %]
    sql_query [% sql.query %]

[% END %]

default_user_options
  reception urlize|mail|digest

ttl 360
```

The XML file format should comply with the following rules:

  - The root element is `<list>`.
  - One XML element is mandatory: `<listname>` contains the name of the list. That does not exclude mandatory parameters for list creation ("listname, subject,owner.email and/or owner\_include.source").
  - `<type>`: this element contains the name of template list creation, it is used for list creation on command line with `sympa.pl`. In a family context, this element is no used.
  - `<description>`: the text contained in this element is written in list `info` file (it can be a CDATA section).
  - For other elements, the name is the name of the var to assign in the list creation template.
  - Each element concerning multiple parameters must have the `multiple` attribute set to `1`, example: `<owner multiple="1">`
  - For composed and multiple parameters, sub-elements are used. Example for the `owner` parameter: `<email>` and `<gecos>` elements are contained in the `<owner>` element. An element can only have homogeneous content.
  - A list requires at least one owner, defined in the XML input file with one of the following elements:

      - `<owner multiple="1"> <email> ... </email> </owner>`
      - `<owner_include multiple="1"> <source> ... </source> </owner_include>`

List families
-------------

See chapter [List families](../customize/basics-families.md).

List creation on command line with `sympa.pl`
---------------------------------------------

This way to create lists is independent of family. The list creator has to choose a profile for the list and put its name in the XML element `<type>`.

Here is a sample command to create one list:.

``` bash
# sympa.pl --create_list --robot mail.example.org \
 --input_file /path/to/my_file.xml
```

The list is created under the `my_robot` robot and the list is described in the file `my_file.xml`. The XML file is described before, see [XML file format](#xml-file-format).

By default, the status of the list created is `open`.

Creating and editing mailing lists using the Web
------------------------------------------------

The management of mailing lists is based on a strict definition of privileges which pertain respectively to the listmaster, to the main list owner, and to basic list owners. The goal is to allow each listmaster to define who can create lists, and which parameters may be set by owners.

### List creation on the web interface

Listmasters are responsible for validating new mailing lists and, depending on the configuration chosen, might be the only ones who can fill out the create list form.The listmaster is defined in `sympa.conf` and others are defined at the virtual host level. By default, any authenticated user can request a list creation, but newly created lists are then validated by the listmaster.

The list rejection message and list creation notification message are both templates you can customize (`list_rejected.tt2` and `list_created.tt2`).

### Who can create lists on the web interface

(Work in progress)

### Typical list profile and web interface

As on command line creation, the list creator has to choose a list profile and to fill in the owner's email and the list subject together with a short description. But in this case, you do not need any XML file. Concerning these typical list profiles, they are described before, see "[Typical list profile](#typical-list-profile)". You can check available profiles. On the web interface, another way to control publicly available profiles is to edit the `create_list.conf` file (the default for this file is in the [``$DEFAULTDIR``](../layout.md#defaultdir) directory, and you may create your own customized version in [``$SYSCONFDIR``](../layout.md#sysconfdir)). This file controls which of the available list templates are to be displayed. Example:

``` code
## This sample hides the public_anonymous create_list template
public_anonymous hidden
defaults read
```

### customize `create_list_request.tt2`

The list creation form is in a template named create\_list\_request.tt2 . You may modify this template in order to some other input that will be used to modify the created list. Any new input variable will be catched by wwsympa.fcgi and available in the tt2 hash \[% custom\_input %\] when using the list template to create teh list.

Example:
``` code
## This is an html part added in create_list-request.tt2
<input type="text" name="custom_input.ldap_group" />
```

In `config.tt2` you may use
``` code
… 
include_ldap_query
  host ldap.foo.edu
  suffix ou=accounts,dc… 
  filter (&(isMemberOf=[% custom_input.ldap_group %]))

…
```

For more details on tt2 customization, templates path and so on, see "~~[Templates](../customize/basics-templates.md)~~".

### List editing

(Work in progress)

**Parameter** can be any list config parameter or the name of a template (thus controlling the edition of the template through the *customize* web admin feature. You can refer to a subentry of a structured list parameter using the '.' as a separator (examples: **owner.email** or **web\_archive.quota**). **default** is a reserved parameter name that means *any other parameter*.

(Work in progress)

Using `edit_list.conf`, you can define privileges on the following lists parameters:

  - All the lists config parameters ([those used in the config file](../man/list_config.5.md)),

  - Any file used in list context and likely to be edited through the web interface:

      - homepage
      - info
      - welcome.tt2
      - rejection messages
      - invite.tt2
      - remind.tt2
      - message.footer
      - message.header
      - bye.tt2
      - removed.tt2
      - your\_infected\_msg.tt2

----
Notes:

Starting Sympa 6.1.10, a **unique** exception exists in the matching between file names and edition rights in the edit\_list.conf: the "info" term.

"info" represents both a list parameter and a list file:

  - The info **list parameter** controls the authorization scenario that will be used to know who can view the lists information text, both in the list welcome page and by using the "info" command. the value to use in the edit\_list.conf to control who can edit this parameter's value is "info".

  - The info **file** contains the text to be displayed when somebody requests to see the list's informations. This is the content whose access is controlled by the info **list parameter**. the value to use in the edit\_list.conf to control who can edit this parameter's value is "info.file".

Obviously, there is no reason why the exact same people would have the same rights on these two informations. But as they have the same name, one must use differetn keys in the "edit\_list.conf" file to discriminate them.

In the following example, an owner and a privileged owner can both edit the info file, but only the privileged owner can change the info scenario to be used (the owner is only allowed to read it):

``` code
info.file    owner,privileged_owner        write
info        privileged_owner        write
info        privileged_owner        read
```

----

(Work in progress)

Concerning list editing in a family context, see "~~[Editing list parameters in a family context](../customize/basics-families#editing-list-parameters-in-a-family-context)~~".

Removing a list
---------------

You can remove (close) a list either from the command line or by using the web interface.

`sympa.pl` provides an option to remove a mailing list, see the example below:

``` bash
# sympa.pl --close_list=mylist@mydomain
```

Privileged owners can remove a mailing list through the list administration part of the web interface. Removing the mailing list consists in removing its subscribers from the database and setting its status to *closed*. Once removed, the list can still be restored by the listmaster; list members are saved in a `subscribers.closed.dump` file.
