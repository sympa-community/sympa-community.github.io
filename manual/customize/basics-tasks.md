---
title: 'Tasks'
prev: basics-scenarios.md
up: ../customize.md#customization-basics
next: basics-i18n.md
---

Tasks
=====

Definition
----------

A task is a sequence of simple actions which realize a complex routine. It is executed in the background by the task manager daemon and allows listmasters to automate the processing of recurring tasks. For example, a task sends to a list's subscribers a message to remind them of their subscription on a yearly basis.

A task is created with a task model. It is a text file which describes a sequence of simple actions. It may have different versions (for instance reminding subscribers every year or semester). A task model file name has the following format: `<model name>.<model version>.task`. For instance `remind.annual.task` or `remind.semestrial.task`.

Sympa provides several task models stored in the [``$DEFAULTDIR``](../layout.md#defaultdir)`/tasks` directory. Others can be designed by listmasters.

A task can be either global or related to a list.

----
Note:

  * On Sympa 6.2.36 or earlier, global and list tasks were placed in different
    directories.  See below for details.

----

Basics
------

Though you can freely modify the content of any model task and create as much versions of each model as you need, there is no simple way to add a *new* model name.

This is due to the fact that, in order to use a task (whatever its focus is, global or list related), you must:

  - Ensure that the task model file exists in the pertinent location;

  - Set the configuration parameter that will make the task manager use the right task model file.

Both the model name and the parameter are hard-coded in Sympa for each task model. Consequently, if you want to create a brand new task model (and not a new version of an existing model), you must modify Sympa to add this new task and its associated parameter.

### Note: confusing denominations

Two different objects used by the task manager can have the same name;

  - the *model* name referred above, used in the task model file name composition;

  - the *command* name used *inside* the task model file to call a functionality offered by the task manager.

***These are two completely different things*** but for historical reasons some have exactly the same name. For example there is a model name called `sync_include` AND a command called `sync_include()`. You can call the `sync_include()` command inside the `sync_include` model but you don't have to.

Paradoxically, you can perfectly create a task model called `sync_include` which will never call the `sync_include()` command. For your own good, though, you shouldn't.

Creating tasks
==============

As stated above, tasks can be either global or list-related. Both these focus have their own tasks models. We will present here, for each one, the location of the task model files, the name of the model to be used as well as their common signification, and the parameters defining which version of the model will be used.

List task creation
------------------

### List model paths

You define in the list configuration file the model and the version you want to use. Then the task manager daemon will automatically create the task by looking for the appropriate model file in different directories in the following order:

  - [``$EXPLDIR``](../layout.md#expldir)`/<list path>/tasks`;

  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/<mail domain name>/tasks/`;

  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/tasks/`;

  - [``$DEFAULTDIR``](../layout.md#defaultdir)`/tasks/`.

----
Note:

  * On Sympa 6.2.36 or earlier, directory `list_task_models` was used
    instead of `tasks` for list tasks.

----

<!---
See also "[Tasks](basics-list-config.md#tasks)" in "Configuration for each list" to know more about standard list models provided with Sympa.
--->

### List model names

You can use two model names in your list task model files :

  - `remind` : generally used to remind subscribers that they subscribed to this list;

  - `sync_include`: synchronize users with data sources (see also "[Data sources](../customize/data-sources.md)").

### List model version definition parameters

List tasks are defined through specific parameters in the `config` file. As of this writing, the following parameters are available :

  - [remind_task](/gpldoc/man/sympa_config.5.html#remind_task) for `remind` task;

  - [ttl](/gpldoc/man/sympa_config.5.html#ttl) for `sync_include` task.

Global task creation
--------------------

### Global model paths

The task manager daemon checks if a version of a global task model exists in different directories in the following order:

  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/tasks/`;

  - [``$DEFAULTDIR``](../layout.md#defaultdir)`/tasks/`.

----
Note:

  * On Sympa 6.2.36 or earlier, directory `global_task_models` was used
    instead of `tasks` for global tasks.

----

### Global model names

You can use the following model names for global tasks. The description below corresponds to the default action of these tasks. By modifying these files, you will alter the actions performed:

  - `expire_bounce`: this task resets bouncing information for addresses not bouncing in the last 10 days after the latest message distribution;

  - `purge_orphan_bounces`: deletes bounce archive for unsubscribed users;

  - `eval_bouncers`: evaluates all bouncing users for all lists, and fill the field `bounce_score_suscriber` in table `suscriber_table` with a score. This score allows the auto-management of bouncing users;

  - `process_bouncers`: executes configured actions on bouncing users, according to their score. The association between score and actions has to be done in List configuration. This parameter defines the frequency of execution for this task;

  - `remind`: reminds subscribers that they are suscribed to this list;

  - `purge_user_table`: removes entries in the `user_table` table that have no corresponding entries in the `subscriber_table` table;

  - `purge_logs_table`: removes all the logs from the database;

See the synonyms commands below. These models are usually employed to apply these commands.

### Global model version definition parameters

The version of a global model to be used is specified in [``sympa.conf``](../layout.md#config). These are the parameters you can set in this configuration file:

  - [`expire_bounce_task`](/gpldoc/man/sympa_config.5.html#expire_bounce_task): `expire_bounce` model definition;

  - [`purge_orphan_bounces_task`](/gpldoc/man/sympa_config.5.html#purge_orphan_bounces_task): `purge_orphan_bounces` model definition;

  - [`eval_bouncers_task`](/gpldoc/man/sympa_config.5.html#eval_bouncers_task) : `eval_bouncers` model definition;

  - [`process_bouncers_task`](/gpldoc/man/sympa_config.5.html#process_bouncers_task) : `process_bouncers` model definition;

  - [`default_remind_task`](/gpldoc/man/sympa_config.5.html#default_remind_task) : the `remind` model used by default in lists;

  - [`purge_user_table_task`](/gpldoc/man/sympa_config.5.html#purge_user_table_task) : `purge_user_table` model definition;

  - [`purge_logs_table_task`](/gpldoc/man/sympa_config.5.html#purge_logs_table_task) : `purge_logs_table` model definition;

The `sync_include` model is an exception, as it doesn't have a single dedicated configuration parameter.

The ``sync_include`` task
--------------------------

An exception in the realm of tasks in Sympa, the `sync_include` task accepts one and only one model : `sync_include.ttl.task`. It's useless to try and create other versions of this task, they will be ignored. There exist a configuration parameter related to `sync_include`, though, but it doesn't set the model used. It is the [ttl](/gpldoc/man/sympa_config.5.html#ttl) parameter. It will just set the length of time between two synchronizations.

Model files constitution
========================

This section describes what you can write in your model files and how to define the action processed during tasks implementation.

Model file format
-----------------

Model files are composed of comments, labels, references, variables, date values and commands. All those syntactical elements are composed of alphanumerics (`0-9a-zA-Z`) and underscores (`_`).

  - Comment lines begin by `#` and are not interpreted by the task manager.

  - Label lines begin by `/` and are used by the next command (see below).

  - References are enclosed between brackets `[` ... `]`. They refer to a value depending on the object of the task (for instance `[list.name]`). Those variables are instantiated when a task file is created from a model file. The list of available variables is the same as for templates plus `[creation_date]` (see below).

<!---
(see "[Templates](basics-list-config.md#templates)")
--->

  - Variables store results of some commands and are parameters for others. Their names begin with '@'.

  - A date value may be written in two ways:

      - Absolute dates follow the format: xxxxYxxMxxDxxHxxMin. Y is the year, M the month (1-12), D the day (1-28, 30 or 31, leap-years are not managed), H the hour (0-23), Min the minute (0-59). H and Min are optional. For instance, 2001y12m4d44min is the 4th of December 2001 at 00h44.

      - Relative dates use the `[creation_date]` or `[execution_date]` references. `[creation_date]` is the date when the task file is created, `[execution_date]` when the command line is executed. A duration may follow with the '+' or '-' operators. The duration is expressed like an absolute date whose all parameters are optional. Examples: `[creation_date]`, `[execution_date]+1y`, `[execution_date]-6m4d`.

  - Command arguments are separated by commas and enclosed between parenthesis `(` ... `)`.

Here is the list of the currently available commands:

  - `stop ()`

    Stops the execution of the task and deletes the task file;

  - `next (<date value>, <label>)`

    Stops the execution. The task will go on at the date value and begin at the label line;

  - `@deleted_users = delete_subs (@<user_selection>)`

    Deletes the `@user_selection` email list and stores user emails successfully deleted in `@deleted_users`;

  - `send_msg (<@user_selection>, <template>)`

    Sends the template message to emails stored in `@user_selection`;

  - `@user_selection = select_subs (<condition>)`

    Stores emails which match the condition in `@user_selection`. See [Authorization Scenarios](basics-scenarios.md) to know how to write conditions. Only available for list models;

  - `create (global | list (<list name>), <model type>, <model>)`

    Creates a task for object with model file `~model type.model.task`;

  - `purge_logs_table()`

    Removes all logs from database;

  - `purge_user_table()`

    Removes from database users which have no role and no subscription;

  - `purge_orphan_bounces()`

    Cleans bounces by removing the unsubscribed users' archive;

  - `eval_bouncers()`

    Evaluates all bouncing users of all lists and gives them a score from 0 to 100. (`0` is for non bouncing users and `100` is for users who should be removed);

  - `process_bouncers()`

    Executes actions defined in list configuration on all bouncing users, according to their score;

  - `expire_bounce(<number_of_days>)`

    For all the lists, removes bouncing address from the bounce list if the last recorded bounce is older than `<number_of_days>` days before the last distribution date;

  - `rm_file(<variable>)`

    Deletes the file whose path is given as argument;

  - `exec(<script>)`

    Executes the script contained by the file whose path is given as argument;

  - `sync_include()`

    Updates the list of users from a database or an LDAP directory.

Model files may have a [scenario-like title](basics-scenarios.md#scenario-title) line at the beginning.

When you change a configuration file by hand, and a task parameter is created or modified, it is up to you to remove existing task files in the `task/` spool if needed. Task file names have the following format:

`<date>.<label>.<model name>.<list name | global>` where:

  - `date` represents the time when the task is executed, it is an epoch date;

  - `label` states where in the task file the execution begins. If empty, it starts at the beginning.

Model file examples
-------------------

You will find plenty of examples in the
[``$DEFAULTDIR``](../layout.md#defaultdir)`/tasks` directory.
Such examples look like:

  - `remind.annual.task`

``` code
title reminder message sent to subscribers weekly

/INIT
next([execution_date] + 1w, EXEC)

/EXEC
@selection = select_subs (older([execution_date]))
send_msg (@selection, remind)
next([execution_date] + 1w, EXEC)
```
