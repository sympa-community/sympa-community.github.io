---
title: 'List families'
prev: basics-i18n.md
up: ../customize.md#customization-basics
next: ../customize.md#customizing-sympa-services
---

List families
=============

A list can have from three up to dozens of parameters. Some listmasters need to create a set of lists that have the same profile. In order to simplify the apprehension of these parameters, list families define a lists typology. Families provide a new level for defaults: in the past, defaults in Sympa were global and most sites using Sympa needed multiple defaults for different groups of lists. Moreover, families allow listmasters to delegate a part of configuration list to owners, in a controlled way according to family properties. Distribution will provide defaults families.

Family concept
--------------

A family provides a model for all of its lists. It is specified by the following characteristics:

  - a list creation template providing a common profile for each list configuration file;
  - a degree of independence between the lists and the family: list parameters editing rights and constraints on these parameters can be `free` (no constraint), `controlled` (a set of available values defined for these parameters) or `fixed` (the value for the parameter is imposed by the family). That prevents lists from diverging from the original and it allows list owner customizations in a controlled way;
  - a filiation kept between lists and family all along the list life: family modifications are applied on lists while keeping listowners customizations.

Here is a list of operations performed on a family:

  - definition: definition of the list creation template, the degree of independence and family customizations;
  - instantiation: list creation or modifications of existing lists while respecting family properties. The set of data defining the lists is an XML document;
  - modification: modification of family properties. The modification is effective at the next instantiation time and has consequences on every list;
  - closure: closure of each list;
  - adding a list to a family;
  - closing a family list;
  - modifying a family list.

Using family
------------

### Definition

Families can be defined at the robot level, at the site level or on the distribution level (where default families are provided). So, you have to create a sub directory named after the family's name in a `families` directory:

  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/families/`_my family_
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`_my domain_`/families/`_my family_

In this directory, you must provide the following files:

  - `config.tt2` (mandatory);
  - `param_constraint.conf` (mandatory);
  - `edit_list.conf`;
  - `automatic_lists_description.conf`;
  - `info.tt2`;
  - other customizable files.

#### `config.tt2`

This is a list creation template, this file is mandatory. It provides default values for parameters. This file is an almost complete list configuration, with a number of missing fields (such as owner email) to be replaced by data obtained at the time of family instantiation. It is easy to create new list templates by modifying existing ones.
See "[Templates](basics-templates.md#list-template-files)".

Example:

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
  [% IF o.gecos -%]
  gecos [% o.gecos %]
  [% END %]

[% END %]
[% IF editor %]
   [% FOREACH e = editor %]
editor
  email [% e.email %]

   [% END %]
[% END %]

[% IF sql %]
include_sql_query
  db_type [% sql.type %]
  host [% sql.host %]
  user [% sql.user %]
  passwd [% sql.pwd %]
  db_name [% sql.name %]
  sql_query [% sql.query %]

[% END %]
ttl 360
```

#### `param_constraint.conf`

This file is mandatory. It defines constraints on parameters. There are three kinds of constraints:

  - "free" parameters: no constraint on these parameters, they are not written in the `param_constraint.conf` file.
  - "controlled" parameters: these parameters must select their values in a set of available values indicated in the `param_constraint.conf` file.
  - "fixed" parameters: these parameters must have the imposed value indicated in the `param_constraint.conf` file.

The parameters constraints will be checked at every list loading.

**WARNING**: Some parameters cannot be constrained, they are: `msg_topic.keywords` (see [`msg_topic`](/gpldoc/man/list_config.5.html#msg_topic)), `owner_include.source_parameter` (see [`owner_include`](/gpldoc/man/list_config.5.html#owner_include)) and `editor_include.source_parameter` (see [`editor_include`](/gpldoc/man/list_config.5.html#editor_include)). About `digest` parameter (see [`digest`](/gpldoc/man/list_config.5.html#digest)), only days can be constrained.

Example:

``` code
lang                fr,us
archive.period      days,week,month
visibility          conceal,noconceal
shared_doc.d_read   public
shared_doc.d_edit   editor
```

#### `edit_list.conf`

This is an optional file. It defines which parameters/files are editable by owners. See "[List editing](../admin/list-creation#list-editing)". If the family does not have this file, Sympa will look for the one defined on robot level, server site level or distribution level (this file already exists without family context).
Note that by default, the `family_name` parameter is not writable, you should not change this editing right.

#### `automatic_lists_description.conf`

This file is used if you want to let users create / access to automatic lists using the Sympa web interface. Please, see the [documentation related to this functionnality](friendly-automatic-lists.md).

#### Common list files

You can parse several files at list creation, later used by the list. Here are
the files you can parse (See also a note below):

  - `message_footer.tt2`,
  - `message_header.tt2`,
  - `message_footer.mime.tt2`,
  - `message_header.mime.tt2`,
  - `info.tt2`

----
Note:

  * On Sympa 6.2.40 or earlier, files for footer and header should be named:
    `message.footer.tt2`, `message.header.tt2`,
    `message.footer.mime.tt2` and/or `message.header.mime.tt2`.

    These files with older names are also usable on recent version of Sympa,
    but newer names are prefered.

----

These files will be parsed using the list family data defined in the XML file. You can use the same data in these files as in the [config.tt2](#config-tt2) file.

For example, if you add to the family directory a file named `info.tt2` containing the following code :

``` code
List [% listname %] home page

[% description %]
```

If your XML file contain something like this for each list :

``` code
<listname>mylist</listname>
<description>A loooong text describing the purpose of the list</description>
```

Then, once you have instantiated the family, each list will contain an `info` file with the following content:

``` code
List mylist home page

A loooong text describing the purpose of the list
```

Message footers and headers are likely to contain TT2 code themselves (for example, to create ~~[unsubscription links](/manual/message-handling#unsubscription_url)~~ at the bottom of the list messages).

In that case, you can use the capacity, offered by TT2, to define [custom tag delimitors](http://template-toolkit.org/docs/manual/Config.html#section_START_TAG_END_TAG).

Here's an example of such usage. Let's say you want to create lists with a family, and you want that each list automatically adds an unsubscription URL at the bottom of each message. For this, you'll need to use a `message.footer` file in each list.

A file named `message.footer.tt2` was added to the family directory. It contains the following code:

``` code
[% TAGS <+ +> -%]
The subject of the list is "<+ subject +>", click here to unsubscribe : [% 'auto_signoff' | url_abs([listname],{email=>user.email}) %]
<+- TAGS [% %] +>
```

"Subject" corresponds to a tag in the XML file. Let's say it contains a short description of the list.

Once the family has been instantiated, each list directory will contain a message.footer file containing the following code :

``` code
The subject of the list is "create and share our passion of scrap cooking", click here to unsubscribe : [% 'auto_signoff' | url_abs([listname],{email=>user.email}) %]
```

Each time a message is sent to the list (provided you set the [`merge_feature`](/gpldoc/man/list_config.5.html#merge_feature) parameter to `on`), this file will be parsed and allow to display the following text at the bottom of each message:

``` code
The subject of the list is "create and share our passion of scrap cooking", click here to unsubscribe : http://lists.domain.tld/auto_signoff/mylist?email=bob.mcbob%40domain.tld
```

----
Note:

  * The line above is an example for Sympa 6.2 later than Aug 2016. With earlier versions it shouold be:
    ``` code
    [% TAGS <+ +> -%]
    The subject of the list is "<+ subject +>", click here to unsubscribe : [% wwsympa_url %]/auto_signoff/[% listname %]/[% user.escaped_email %]
    <+- TAGS [% %] +>
    ```

----

#### customizable files

Families provide a new level of customization for scenarios (see "[Authorization scenarios](../customize/basics-scenarios.md)"), templates for service messages (see "~~[Site template files](../customize/basics-templates.md#site-template-files)~~") and templates for web pages (see "~~[web template files](../customize/basics-templates.md#web-template-files)~~"). Sympa looks for these files in the following level order: list, family, robot, server site or distribution.

Example of custom hierarchy:

  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/families/`_myfamily_`/mail_tt2/`
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/families/`_myfamily_`/mail_tt2/bye.tt2`
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/families/`_myfamily_`/mail_tt2/welcome.tt2`

### Instantiation

Instantiation allows to generate lists. You must provide an XML file made of list descriptions, the root element being `family` and which is only composed of `list` elements. List elements are described in section [XML file format](../admin/list-creation#xml-file-format). Each list is described by the set of values for affectation list parameters.

Here is a sample command to instantiate a family:

``` code
sympa.pl --instantiate_family my_family --robot samplerobot --input_file /path/to/my_file.xml
```

This means lists that belong to family `my_family` will be created under the robot `my_robot` and these lists are described in the file `my_file.xml`. Sympa will split this file into several XML files describing lists. Each list XML file is put in each list directory.

**``--close_unknown``** option can be added to automatically close undefined lists during a new instantation
**``--quiet``** option can be added to skip the report printed to STDOUT

Example:

``` xml
<?xml version="1.0" ?>
<family>
  <list>
    <listname>liste1</listname>
    <subject>a list example</subject>
    <description/>
    <status>open</status>
    <shared_edit>editor</shared_edit>
    <shared_read>private</shared_read>
    <language>fr</language>
    <owner multiple="1">
      <email>foo@cru.fr</email>
      <gecos>C.R.U.</gecos>
    </owner>
    <owner multiple="1">
      <email>foo@emnsp.fr</email>
    </owner>
    <owner_include multiple="1">
      <source>my_file</source>
    </owner_include>
    <sql>
      <type>oracle</type>
      <host>sqlserv.admin.univ-x.fr</host>
      <user>stdutilisateur</user>
      <pwd>monsecret</pwd>
      <name>les_etudiants</name>
      <query>SELECT DISTINCT email FROM etudiant</query>
    </sql>
  </list>
  <list>
    <listname>liste2</listname>
    <subject>a list example</subject>
    <description/>
    <status>open</status>
    <shared_edit>editor</shared_edit>
    <shared_read>private</shared_read>
    <language>fr</language>
    <owner multiple="1">
      <email>foo@cru.fr</email>
      <gecos>C.R.U.</gecos>
    </owner>
    <owner multiple="1">
      <email>foo@enmsp.fr</email>
    </owner>
    <owner_include multiple="1">
      <source>my_file</source>
    </owner_include>
    <sql>
      <type>oracle</type>
      <host>sqlserv.admin.univ-x.fr</host>
      <user>stdutilisateur</user>
      <pwd>monsecret</pwd>
      <name>les_etudiants</name>
      <query>SELECT DISTINCT email FROM etudiant</query>
    </sql>
  </list>
   ...
</family>
```

Each instantiation describes lists. Compared with the previous instantiation, there are three cases:

  - list creation: new lists described by the new instantiation;
  - list modification: lists already existing but possibly changed because of changed parameters values in the XML file or because of changed family properties;
  - list removal: lists no more described by the new instantiation. In this case, the listmaster must validate his choice on command line. If the list is removed, it is set in status `family_closed`, or if the list is recovered, the list XML file from the previous instantiation is got back to go on as a list modification then.

After list creation or modification, parameters constraints are checked:

  - "fixed" parameter: the value must be the one imposed;
  - "controlled" parameter: the value must be one of the set of available values;
  - "free" parameter: there is no checking.

diagram

In case of modification (see diagram), allowed customizations can be preserved:

  - (1): for all parameters modified (through the web interface), indicated in the `config_changes` file, values can be collected in the old list configuration file, according to new family properties:
      - "fixed" parameter: the value is not collected,
      - "controlled" parameter: the value is collected only if constraints are respected,
      - "free" parameter: the value is collected;
  - (2): a new list configuration file is made with the new family properties;
  - (3): collected values are set in the new list configuration file.

Notes:

  - For each list problem (as family file error, error parameter constraint, error instantiation, etc.), the list is set in status `error_config` and listmasters are notified. Then they will have to perform any necessary action in order to put the list in use.
  - For each list closure in family context, the list is set in status `family_closed` and owners are notified.
  - For each overwritten list customization, owners are notified.

### Modification

To modify a family, you have to edit family files manually. The modification will be effective while the next instanciation.
**WARNING**: The family modification must be done just before an instantiation. Otherwise, alive lists would not respect new family properties and they would be set in status `error_config` immediately.

### Closure

Closes every list (installed under the indicated robot) of this family: list status is set to `family_closed`, aliases are removed and subscribers are removed from DB (a dump is created in the list directory to allow restoration of the list).

Here is a sample command to close a family:

``` bash
sympa.pl --close_family my_family --robot samplerobot
```

### Adding a list to a list family

Adds a list to the family without instantiating the whole family. The list is created as if it was created during an instantiation, under the indicated robot. The XML file describes the list and the root element is `<list>`. List elements are described in section [List creation on command line with sympa.pl](../admin/list-creation.md#list-creation-on-command-line-with-sympa-pl).

Here is a sample command to add a list to a family:

``` bash
sympa.pl --add_list my_family --robot samplerobot  --input_file /path/to/my_file.xml
```

### Removing a list from a list family

Closes the list installed under the indicated robot: the list status is set to `family_closed`, aliases are removed and subscribers are removed from DB (a dump is created in the list directory to allow restoring the list).

Here is a sample command to close a list family (same as an orphan list):

``` bash
sympa.pl --close_list my_list@samplerobot
```

### Modifying a family list

Modifies a family list without instantiating the whole family. The list (installed under the indicated robot) is modified as if it was modified during an instantiation. The XML file describes the list and the root element is `<list>`. List elements are described in section [List creation on command line with sympa.pl](../admin/list-creation.md#list-creation-on-command-line-with-sympa-pl).

Here is a sample command to modify a list to a family:

``` bash
sympa.pl --modify_list my_family --robot samplerobot --input_file /path/to/my_file.xml
```

### Editing list parameters in a family context

According to file `edit_list.conf`, editing rights are controlled. See "[List editing](../admin/list-creation.md#list-editing)". But in a family context, constraints parameters are added to editing right as it is summarized in this array:

array

Note: in order to preserve list customization for instantiation, every parameter modified (through the web interface) is indicated in the `config_changes` file.

### Family unsubscription

Using a global family message.footer.tt2 file, you can add at the end of each message sent from a family list a global unsubscription link. Here's what you could put in such message.footer.tt2:

``` code
[% TAGS <+ +> -%]
To stop receiving messages from <+ family_config.display +>, click on this link: [% 'family_signoff' | url_abs(['<+ family_config.name +>'],{email=user.email}) %]
<+ TAGS [% %] -+>
```

By clicking this link, the user will be redirected to the Sympa web interface where she will be informed that a confirmation message was just sent to her. If she clicks the confirmation link in this mesasge, she will be removed from all the past and future lists of this family.

----
Note:

  * The line above is an example for Sympa 6.2.54 or later.
    With Sympa 6.2.52 or earlier it should be:

    ``` code
    [% TAGS <+ +> -%]
    To stop receiving messages from <+ family_config.display +>, click on this link: [% wwsympa_url %]/family_signoff_request/<+ family_config.name +>/[% user.escaped_email %]
    <+ TAGS [% %] -+>
    ```

----

