---
title: 'Configuration for each list'
prev: basics-configuration.md
up: ../customize.md#customization-basics
next: basics-templates.md
---

Configuration for each list
===========================

A list has a directory of its own.  Most of configuration files and
control files specific to the list are placed there.
List also has several other directories to provide extensive features.

Directories
-----------

Following directories may be used so that a list will work.
"List directory" must be created at the time of
[list creation](../admin/list-creation.md#list-creation).
The others will be created as their necessities.

  * List directory

    Directory which contains configuration specific to a list (see also
    [below](#configuration-files-under-list-directory)).

    Default location is
    [``$EXPLDIR``](../layout.md#expldir)`/`_mail domain_`/`_list name_`/`.

    ----
    Note:

      * Exceptionally, [``$EXPLDIR``](../layout.md#expldir)`/`_list name_`/`
        is also allowed for primary mail domain (mail domain defined by
        `domain` parameter in [``sympa.conf``](/gpldoc/man/sympa.conf.5.html)).
        However, full path described above is recommended for new
        installation.

    ----

  * Bounce directory

    Base directory of bounce and tracking information (see also
    "[Bounce management](../customize/bounce-management.md)").

    Default location is
    [``$BOUNCEDIR``](../layout.md#bouncedir)`/`_list name_`@`_mail domain_`/`.

  * Archive directory

    Base directory of list archives (see also
    "~~[Archives](../customize/archives.md)~~").

    Default location is
    [``$ARCDIR``](../layout.md#arcdir)`/`_list name_`@`_mail domain_`/`.

  * Pictures directory

    Directory for subscribers' pictures.

    Default location is
    [``$PICTURESDIR``](../layout.md#picturesdir)`/`_list name_`@`_mail domain_`/`.

  * Shared document repository

    Base directory of Shared document repository (see also
    "~~[Shared document repository](../customize/shared-repository.md)~~").

    Location is the subdirectory named `shared/` under list directory
    described above.

Configuration files under list directory
----------------------------------------

There may be following configuration files under list directory.
Among them, `config` file is required.
The others may be created during list creation, or configuration works by
listmasters or owners.

### Main configuration

  * `config`

    The file keeps most of list configuration.
    See also
    "[Configuration files](basics-configuration.md#configuration-files)"
    in "Configuration hierarchy".
    See [`list_config(5)`](/gpldoc/man/list_config.5.html) for details of format and
    configuration parameters.

### `info` and `homepage` files

  * `info`

    Long description of the list.
    Content of this file will be included in welcome message
    (`mail_tt2/welcome.tt2`) sent to new subscribers
    or response to `INFO` ~~[mail command](../mail-commands.md)~~.

    This file contains the text entered as "Description" in list creation page
    of web interface.

  * `homepage`

    Content of this file will be included in list information page of
    web interface.  HTML format is allowed.
    If it does not exist, content of `info` file will be used.

### Message header and footer

  * `message_footer`
  * `message_footer.mime`

    ----
    Note:

      * On Sympa 6.2.40 or earlier, these were named `message.footer`
        and `message.footer.mime`.

    ----

    If either of these files exists, it will be appended at end of
    distributed messages.

    `message_footer.mime` is used only when footer type is "mime" (see also
    [`footer_type`](/gpldoc/man/list_config.5.html#footer_type) parameter).

  * `message_header`
  * `message_header.mime`

    ----
    Note:

      * On Sympa 6.2.40 or earlier, these were named `message.header`
        and `message.header.mime`.

    ----

    Similar to files above, but they will be appended at beginning of
    distributed messages.

### Templates

  * `mail_tt2/`_file name_`.tt2`
  * `web_tt2/`_file name_`.tt2`

    Mail templates (`mail_tt2/`_file name_`.tt2`)
    and web templates (`mail_tt2/`_file name_`.tt2`)
    for specific list may customize templates.
    See also "[Templates](basics-templates.md)".

### Tasks

  * `tasks/`_file name_`.task`

    Task files for specific list.
    Note that, with earlier versions, `list_task_models` directory was used instead of
    `tasks`.
    See "[Tasks](basics-tasks.md)" for details.

### Data inclusion files

  * `data_sources/`_file name_`.incl`

    Data inclusion files for specific list may customize data source definitions.
    See also "[Data sources](../customize/data-sources.md)".

Control files
-------------

### Dump files

----
Note:

  * Feature of dump files was introduced on Sympa 6.2.33b.1.

----

  * `owner.dump`
  * `editor.dump`
  * `member.dump`

    These files include snapshots of list users:
    `owner.dump` for list owners, `editor.dump` for moderators and
    `member.dump` for subscribers.

#### Creation, opening, modifying, closing or restoring lists

When a list is created using list creation template
(see "[List creation](../admin/list-creation.md#list-creation)"), dump files
are created and imported
to backend database immediately.

When a family list is modified (see
"[Modifying a family list](basics-families.md#modifying-a-family-list)"),
dump files including new owners and moderators are created, and owners and
moderators in backend database are replaced with content of dump files.

When a list is closed (see
"[Removing a list](../admin/list-creation.md#removing-a-list)"),
list users are exported into dump files, and users are
removed from backaend database.  When the list is restored by listmaster,
users in dump files are imported again.

#### Handling dump files with command line interface

Additionally, administrator can export users to dump files using command line
`sympa.pl --dump_users` anytime, or import dump files using
`sympa.pl --restore_users` (see [`sympa.pl(1)`](/gpldoc/man/sympa.1.html)) anytime.
`sympa.pl --dump_users` does not remove users in database backend.

### Other files

In general, these files should not be modified manually.

  * `stats`

    `stats` file under list directory contains accumulated amount of message
    delivery.  It contains following four integer numbers separated by space
    characters:

      1. Number of messages the list has distributed.
         This number is assigned as value of `list.sequence` template variable
         used with [`custom_subject`](/gpldoc/man/list_config.5.html#custom_subject)
         parameter.
      2. Total number of messages the list has distributed.
      3. Sum of message sizes the list has distributed.
      4. Total sum of message sizes the list has distributed.

    ----
    Note:

      * This file on earlier versions of Sympa might contain additional
        information.  Format of this file may be changed in the future.

    ----

  * `msg_count`

    `msg_count` file under list directory contains number of messages distributed
    by days.  Its content will be imported in database, and shown in
    "List statistics" page of web interface.

  * `config_changes`

    `config_changes` file under list directory tracks modification of list
    configuration by users, if the list belongs to family.

    (Work in progress)

