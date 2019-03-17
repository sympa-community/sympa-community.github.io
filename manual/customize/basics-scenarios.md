---
title: 'Authorization scenarios'
prev: basics-templates.md
up: ../customize.md#customization-basics
next: basics-tasks.md
---

Authorization scenarios
=======================

The authorization scenario (or simply "scenario") is a small configuration
language. It describes authorization for each operation of Sympa based on
various conditions such as actors and authentication methods.

Location of scenario file
-------------------------

Each scenario file is named as "_function_`.`_name_". The _function_
corresponds to a function to be authorized, and the _name_ distinguishes the
policy.  Usually, some scenario files with different names are prepared for
each function.

For example, a scenario file

> `subscribe.open`

may be used to make subscription of a list be open. To do such,

  * Edit list [``config``](/gpldoc/man/list_config.5.html) file to add the following
    setting:
    ```
    subscribe open
    ```
  * In the web interface, choose "for anyone without authentication (open)"
    option on "Who can subscribe to the list (subscribe)" in the list
    configuration page.

To look for a particular scenario, Sympa searches following directories in
order (see also
"[Configuration files](basics-configuration.md#configuration-files)"):

  - [``$EXPLDIR``](../layout.md#expldir)`/`_list path_`/scenari`
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`_virtual host_`/scenari`
  - [``$SYSCONFDIR``](../layout.md#sysconfdir)`/scenari`
  - [``$DEFAULTDIR``](../layout.md#defaultdir)`/scenari`

You can modify Sympa behavior if you create a new scenario with the same name as one of the scenario already included in the distribution but with a location related to the target site, robot or list. You can also add a new scenario ; it will automatically add an accepted value for the related parameter.

See also "[Hiding scenario files](#hiding-scenario-files)".

----
Notes:

  * When customizing a scenario for your own site, robot or list, don't modify [``$DEFAULTDIR``](../layout.md#defaultdir)`/scenari` content or the next Sympa update will overwrite it (you must never modify anything in [``$DEFAULTDIR``](../layout.md#defaultdir) unless you are patching Sympa).

  * When modifying an existing scenario you need to restart Sympa or touch list [`config`](/gpldoc/man/list_config.5.html) file before Sympa use it.

----

Content of scenario file
------------------------

Basically, a scenario file is composed of one or more titles on the
first lines and a set of rules on the following lines. For example:
```
title.en-US deletion performed only by list owners, need authentication
title.es eliminacin reservada slo para el propietario, necesita autentificacin
title.fr suppression réservée au propriétaire avec authentification

is_owner([listname],[sender])  smtp       -> request_auth
is_listmaster([sender])        smtp       -> request_auth
true()                         md5,smime  -> do_it
```

### Scenario title

The first line of a scenario file can contains its own title. This is the text that will later appear in the drop-down menu of your management web interface. This title can be just plain text:
``` code
title Restricted to subscribers
```

It can also be set to be internationalized:
``` code
title Restricted to subscribers
title.en-US Restricted to subscribers
title.es restringido a subscriptores
title.fr limité aux abonnés
```
or
``` code
title.gettext Restricted to subscribers
```
In the first example, appropriate `title.`_lang_ line will be used for
title according to users' language context (see also
"[Language tag](basics-i18n.md#language-tag)"). If no appropriate line is
found, `title` will be used as default.
In the second, the string following `title.gettext` will be translated to
the users' language using Sympa's translation catalog. If there are no entry in
catalog, the string itself will be used.

### Rules

----
Note:

  * About detailed description on rules, see also
    [sympa_scenario(5)](/gpldoc/man/sympa_scenario.5.html) manual page.

----

One or more lines for rules follow to title. Each rule has the following syntax:

> *condition* *authentication_methods* `->` *action*

Rules are evaluated in order, and once a rule matching with given
*condition* and *authentication method* is found, *action* will be returned
and subsequent rules will no longer be evaluated.

#### Conditions

A condition is the term with optional arguments.  Each argument may be a
variable with a given value according to context, or a literal string.

For example:

  - `is_owner([listname],[sender])` succeeds if the user that requested the operation
    is the owner of the list.

  - `true()` succeeds always.

  - As an argument, a variable `[custom_vars->`*variable name*`]` allows you
    to introduce [custom parameters](../customize/custom-parameters.md)
    in your scenario.

You can also create custom conditions by writing a Perl module:
See "[Custom scenario conditions](custom-scenario-conditions.md)" for details.

#### Authentication methods

You can specify four different authentication methods to base your rules on (ordered from lower to higher levels):

  - `smtp`
  - `dkim`
  - `md5`
  - `smime`

If the specified authentication method was not used, a higher level of authentication methods can be requested.

These methods take a different meaning if you consider them in a web or mail context.  For example with the scenario `send` which will be evaluated when people try to send a message to a list:

  - If the message is sent through the web interface, Sympa will verify the identity of the sender based on information (login/password, certificate, etc.) provided by one of [authentication mechanisms](authentication-web.md#authentication-mechanisms).

  - If it is sent by the mail client, the authentication is based on whatever authentication method the user's email client associated with the SMTP message (S/MIME signature, From field, etc.).

However *the same scenario file will be used in both cases.* That means that the same authentication method will be used, whichever context we're in. It is consequently important to understand what interpretation to give to each authentication method according to the context.

Here is a description of what is evaluated to authenticate the user depending of the context: web or mail.

| Method  | Mail context | Web context |
|---------|--------------|-------------|
| `smtp`  | sender ("`From:`" field etc.) in the message header | anonymous user (`nobody`) |
| `dkim`  | valid DKIM signature of the message | *Nothing - unused in web context* |
| `md5`   | confirmation/approval with the authentication key in the message [1] | authentication mechanisms with username/password |
| `smime` | valid S/MIME signature of the message [2] | TLS client authentication with X.509 certificate installed in the user's browser |

  - [1] `md5` will be used, in a mail context, when users answer to an authentication request, or when moderators moderate a message by replying to a moderation request mail.
  - [2] Sympa also deals with S/MIME encrypted messages.

In most cases, `smtp` or `dkim` will be used for mails, and `md5` for the web.

#### Actions

An action is one of following keywords:

  * `do_it`

    Allows operation.

  * `listmaster`

    Same as `do_it` but makes the status of newly created list "pending".
    Only for `create_list` function.  

  * `request_auth`

    Requests the users of their own to authenticate (confirm) performing
    operation.

  * `editor`

    Forwards the message to the list moderators.
    Only for `send` function.

  * `editorkey`

    Holds the message for moderation and sends notification to the moderators.
    Only for `send` function.

  * `owner`

    Requests the list owners to authenticate performing operation.
    Only for `subscribe` and `unsubscribe` functions.

  * `reject`

    Denies operation.

Some actions may have modifiers, for example:
> `editorkey,quiet`

> `do_it,notify`

> `reject(reason='subscribe_closed')`

> `reject(tt2='custom_response')`

  * With `quiet` modifier, no notification is sent to the message sender.

    For example, if a scenario rule is applied and result in
    `editorkey,quiet`, the sender of the message will not receive the
    automatic information message telling them that their message has been
    forwarded to the list moderator.

  * `notify` modifier sends a report to the list owner, if it will not be sent by
    default.

  * `(reason=...)` modifier returns a key in `mail_tt2/report.tt2` template as
    the reason of rejection.

    ----
    Note:

      * On Sympa 6.2.18 or earlier, `mail_tt2/authorization_reject.tt2`
        template was referred instead.

    ----

  * `(tt2=...)` modifier sends the user a rejection message generated from
    specified template (extension `.tt2` will be added).

This is an important issue to prevent *backscatter* messages. Backscatter messages are messages you receive as an automatic answer to a message you never sent. The following web page give you more details :

  - [http://www.spamresource.com/2007/02/backscatter-what-is-it-how-do-i-stop-it.html](http://www.spamresource.com/2007/02/backscatter-what-is-it-how-do-i-stop-it.html)

  - [http://en.wikipedia.org/wiki/Backscatter](http://en.wikipedia.org/wiki/Backscatter)

<!---
Sympa versions 6.0 and later provide a better mechanism to prevent backscatter. See "~~[](antispam.md)~~".
--->

Named Filters
-------------

At the moment, Named Filters are only used in authorization scenarios. They enable to select a category of people who will be authorized or not to realize some actions.

As a consequence, you can grant privileges in a list to people belonging to an LDAP directory, an SQL database or a flat text file, thanks to an authorization scenario.

Note that only a subset of variables available in the scenario context are available here (including `[sender]` and `[listname]`).

### LDAP Named Filters Definition

People are selected through an LDAP filter defined in a configuration file. This file must have the extension '.ldap'. It is stored in [``$SYSCONFDIR``](../layout.md#sysconfdir)`/search_filters/`.

You must provide some information in order to create a LDAP Named Filter:

  - `host`

    A list of host:port LDAP directories (replicates) entries.

  - `suffix`

    Defines the naming space covered by the search (optional, depending on the LDAP server).

  - `filter`

    Defines the LDAP search filter ([RFC 4515](https://tools.ietf.org/html/rfc4515)). But you must absolutely take into account the first part of the filter which is: `(mail_attribute = [sender])`, as shown in the example. You will have to replace `mail_attribute` by the name of the attribute for the email. Sympa checks whether the user belongs to the category of people defined in the filter.

  - `scope`

    By default, the search is performed on the whole tree below the specified base object. This may be changed by specifying a scope:

      - `base`: search only the base object.

      - `one`: search the entries immediately below the base object.

      - `sub`: search the whole tree below the base object. This is the default option.

  - `bind_dn`

    If anonymous bind is not allowed on the LDAP server, a DN and password can be used.

  - `bind_password`

    This password is used, combined with the `bind_dn` above.

`example.ldap`: we want to select the teachers of mathematics in the University of Rennes 1 in France:

``` code
host        ldap.univ-rennes1.fr:389,ldap2.univ-rennes1.fr:390
suffix      dc=univ-rennes1.fr,dc=fr
filter      (&(canonic_mail = [sender])(EmployeeType = prof)(subject = math))
scope       sub
```

### SQL Named Filters Definition

People are selected through an SQL filter defined in a configuration file. This file must have the extension '.sql'. It is stored in [``$SYSCONFDIR``](../layout.md#sysconfdir)`/search_filters/`.

To create an SQL Named Filter, you have to configure SQL host, database and options, the same way you did it for the main Sympa database in `sympa.conf`. Of course, you can use different database and options. Sympa will open a new Database connection to execute your statement.

Please refer to section "[Database related](/gpldoc/man/sympa.conf.5.html#database-related)" for a detailed explanation of each parameter.

Here, all database parameters have to be grouped in one `sql_named_filter_query` paragraph.

  - `db_type`

    Format: `db_type MySQL|Oracle|PostgreSQL|SQLite`; Database management system used. Mandatory and case sensitive.

  - `db_host`

    Database host name. Mandatory.

  - `db_name`

    Name of database to query. Mandatory.

  - `statement`

    Mandatory. The SQL statement to execute to verify authorization. This statement must returns 0 to refuse the action, or anything else to grant privileges. The `SELECT COUNT(*)...` statement is the perfect query for this parameter. The keyword in the SQL query will be replaced by the sender's email.

  - Optional parameters

    Please refer to main `sympa.conf` section for description.

      - [`db_user`](/gpldoc/man/sympa.conf.5.html#db_user)
      - [`db_password`](/gpldoc/man/sympa.conf.5.html#db_password)
      - [`db_options`](/gpldoc/man/sympa.conf.5.html#db_options)
      - [`db_env`](/gpldoc/man/sympa.conf.5.html#db_env)
      - [`db_port`](/gpldoc/man/sympa.conf.5.html#db_port)
      - [`db_timeout`](/gpldoc/man/sympa.conf.5.html#db_timeout)

`example.sql`: we want to select the teachers of mathematics in the University of Rennes 1 in France:

``` code
sql_named_filter_query
  db_type         mysql
  db_name         people
  db_host         dbserver.rennes1.fr
  db_user         sympa
  db_passwd       pw_sympa_mysqluser
  statement       SELECT count(*) as c FROM users WHERE mail=[sender] AND EmployeeType='PROFESSOR' AND department='mathematics'
```

### Search condition

The search condition is used in authorization scenarios.

The syntax of this rule is:

``` code
search(example.ldap)      smtp,smime,md5    -> do_it
search(blacklist.txt)     smtp,smime,md5    -> do_it
```

The variable used by `search` is the name of the LDAP configuration file or a txt matching enumeration.

Note that Sympa processes maintain a cache of processed search conditions to limit access to the LDAP directory or SQL server; each entry has a lifetime of one hour in the cache.

When using the '.txt' file extension, each line is used to try to match the sender email address. You can use the "\*" character as a wild card to replace any string.

Here is an example of such a file:

``` code
david.verdin@renater.fr
*salaun*
```

With such a file, the rule would be true for the following email addresses:

  - david.verdin@renater.fr
  - salaun@renater.fr
  - O.salaun@renater.fr

It would be false for the following email addresses :

  - verdin@renater.fr
  - olivier.sala@renater.fr

This feature is used by the blacklist implicit scenario rule (see "[Blacklist](/gpldoc/man/sympa.conf.5.html#use_blacklist)").

The method of authentication does not change.

Scenario inclusion
------------------

Scenarios can also contain includes:

``` code
include commonreject
match([sender], /cru\.fr$/)          smtp,smime -> do_it
true()                               smtp,smime -> owner
```

In this case, Sympa applies recursively the scenario named `include.commonreject` before introducing the other rules. This possibility was introduced in order to facilitate the administration of common rules.

### Scenario implicit inclusion

You can define a set of common scenario rules, used by all lists. `include.<action>.header` is automatically added to evaluated scenarios. Note that you will need to restart Sympa processes to force reloading of list config files.

Blacklist implicit rule
-----------------------

For each service listed in parameter [`use_blacklist`](/gpldoc/man/sympa.conf.5.html#use_blacklist), the following implicit scenario rule is added at the beginning of the scenario:

``` code
search(blacklist.txt)  smtp,md5,dkim,smime -> reject,quiet
```

The goal is to block messages or other service requests from unwanted users. The blacklist can be defined for the robot or for the list. At the list level, the blacklist is to be managed by the list owner or the list moderator via the web interface.

`spam-status` special scenario
------------------------------

(Work in progress)

Hiding scenario files
---------------------

Because Sympa is distributed with many default scenario files, you may want to hide some of them to list owners (to make list administration menus shorter and readable). To hide a scenario file, you should create an empty file with the `:ignore` suffix. Depending on where this file has been created, it will make it be ignored at either a global, robot or list level.

Example:

By creating [``$SYSCONFDIR``](../layout.md#sysconfdir)`/mail.example.org/scenari/send.intranetorprivate:ignore`, the `intranetorprivate` `send` scenario will be hidden (on the web admin interface), at the `mail.example.org` robot level only.

