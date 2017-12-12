---
title: 'Authorization scenarios'
prev: basics-templates.md
up: ../customize.md#customization-basics
next: basics-tasks.md
---

Authorization scenarios
=======================

(Work in progress)

The function to evaluate scenario is described in section ~~[internals](/internals/index)~~.

Location of scenario file
-------------------------

(Work in progress)

When customizing scenario for your own site, robot or list, don't modify [``$DEFAULTDIR``](../layout.md#defaultdir)`/scenari` content or the next Sympa update will overwrite it (you must never modify anything in [``$DEFAULTDIR``](../layout.md#defaultdir) unless you are patching Sympa). You can modify Sympa behavior if you are creating a new scenario with the same name as one of the scenario already included in the distribution but with a location related to the target site, robot or list. You can also add a new scenario ; it will automatically add an accepted value for the related parameter.

----
Note:

  * When modifying a existing scenario you need to restart Sympa or touch list [`config`](../man/list_config.5.md) file before Sympa use it.

----

Scenario structure
------------------

Basically, a scenario file is composed of a title on the first line and a set of rules on the following lines.

### Scenario title

The first line of a scenario file can contain its title. This is the text that will later appear in the drop-down menu of your administration web interface. This title can be just plain text:

``` code
title Restricted to subscribers
```

It can also be set to be internationalized:

``` code
title.gettext Restricted to subscribers
```

That way, the character string following `title.gettext` can be handled by Sympa internationalization process.

### Rules overview

(Work in progress)

### Rules definition

(Work in progress)

#### Conditions

`custom_vars` allows you to introduce ~~[custom parameters](/manual/customizing#custom_parameters)~~ in your scenario.

(Work in progress)

You can also create custom condition writing Perl module:
See "[Custom scenario conditions](custom-scenario-conditions.md)" for details.

#### Authentication methods

You can specify three different authentication methods to base your rules on: `smtp`, `dkim`, `smime` and `md5`.

**these methods take a different meaning if you consider them in a web or mail context**.

Indeed if you consider, for example, the scenario `send`: it will be evaluated when people try to send a message to a list.

  - If the message is sent through the web interface, Sympa will verify the identity of the sender based on its web authentication information (login/password, certificate, etc.)

  - If it is sent by the mail client, the authentication is based on whatever authentication method the user's email client associated with the SMTP message (S/MIME signature, From field, etc.).

*But the same scenario file will be used in both cases.* That means that the same authentication method will be used, whichever context we're in. It is consequently important to understand what interpretation to give to each authentication method according to the context.

Here is a description of what is evaluated to authenticate the user depending of the context: web or mail.

| Method  | Mail context                               | Web context                                                        |
|---------|--------------------------------------------|--------------------------------------------------------------------|
| `smtp`  | the "From:" field of the message           | *Nothing - unused in web context*                                  |
| `dkim`  | the DKIM signature of the message          | *Nothing - unused in web context*
| `md5`   | the authentication key in the message      | the authentication information provided to Sympa (login/password) |
| `smime` | the S/MIME X509 signature of the email     | An X509 certificate installed in the user's browser                |

Note that `md5` will be used, in a mail context, when users answer to an authentication request, or when editors moderate a message by replying to a moderation request mail.

In most cases, `smtp` or `dkim` will be used for mails, and `md5` for the web.

#### Actions

(Work in progress)

The `quiet` can be part of the scenario action result. When using this option, no notification is sent to the message sender. For example, if a scenario rule is applied and result in `editorkey,quiet` the sender of the message will not receive the automatic information message telling him that his message has been forwarded to the list editor. This is an important issue to prevent *backscatter* messages. backscatter messages are messages you receive as an automatic answer to a message you never sent. The following web page give you more details :

  - [http://www.spamresource.com/2007/02/backscatter-what-is-it-how-do-i-stop-it.html](http://www.spamresource.com/2007/02/backscatter-what-is-it-how-do-i-stop-it.html)

  - [http://en.wikipedia.org/wiki/Backscatter](http://en.wikipedia.org/wiki/Backscatter)

Sympa version 6.0 and later of Sympa provide a better mechanism to prevent backscatter. See ~~[https://www.sympa.org/manual/antispam](https://www.sympa.org/manual/antispam)~~.

### Formal specification of the rules

(Work in progress)

Named Filters
-------------

At the moment, Named Filters are only used in authorization scenarios. They enable to select a category of people who will be authorized or not to realize some actions.

As a consequence, you can grant privileges in a list to people belonging to an LDAP directory, an SQL database or a flat text file, thanks to an authorization scenario.

Note that only a subset of variables available in the scenario context are available here (including \[sender\] and \[listname\]).

### LDAP Named Filters Definition

People are selected through an LDAP filter defined in a configuration file. This file must have the extension '.ldap'. It is stored in [``$SYSCONFDIR``](../layout.md#sysconfdir)`/search_filters/`.

You must give a few information in order to create a LDAP Named Filter:

  - `host`
    A list of host:port LDAP directories (replicates) entries.

  - `suffix`
    Defines the naming space covered by the search (optional, depending on the LDAP server).

  - `filter`
    Defines the LDAP search filter (RFC 2254 compliant). But you must absolutely take into account the first part of the filter which is: `(mail_attribute = [sender])`, as shown in the example. You will have to replace `mail_attribute` by the name of the attribute for the email. Sympa checks whether the user belongs to the category of people defined in the filter.

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
suffix        dc=univ-rennes1.fr,dc=fr
filter        (&(canonic_mail = [sender])(EmployeeType = prof)(subject = math))
scope        sub
```

### SQL Named Filters Definition

People are selected through an SQL filter defined in a configuration file. This file must have the extension '.sql'. It is stored in [``$SYSCONFDIR``](../layout.md#sysconfdir)`/search_filters/`.

To create an SQL Named Filter, you have to configure SQL host, database and options, the same way you did it for the main Sympa database in `sympa.conf`. Of course, you can use different database and options. Sympa will open a new Database connection to execute your statement.

Please refer to section "[Database related](../man/sympa.conf.5.md#database-related)" for a detailed explanation of each parameter.

Here, all database parameters have to be grouped in one `sql_named_filter_query` paragraph.

  - `db_type`
    Format: `db_type mysql|SQLite|Pg|Oracle|Sybase`; Database management system used. Mandatory and case sensitive.

  - `db_host`
    Database host name. Mandatory.

  - `db_name`
    Name of database to query. Mandatory.

  - `statement`
    Mandatory. The SQL statement to execute to verify authorization. This statement must returns 0 to refuse the action, or anything else to grant privileges. The `SELECT COUNT(*)...` statement is the perfect query for this parameter. The keyword in the SQL query will be replaced by the sender's email.

  - Optional parameters
    Please refer to main `sympa.conf` section for description.

      - `db_user`

      - `db_password`

      - `db_options`

      - `db_env`

      - `db_port`

      - `db_timeout`

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

When using the '.txt' file extension, each line is used to try to match the sender email address. You can use the "\*" character as a joker to replace any string.

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

This feature is used by the blacklist implicit scenario rule (see "[Blacklist](../sympa.conf.5.md#use_blacklist)").

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

Scenario implicit inclusion
---------------------------

You can define a set of common scenario rules, used by all lists. `include.<action>.header` is automatically added to evaluated scenarios. Note that you will need to restart Sympa processes to force reloading of list config files.

Blacklist implicit rule
-----------------------

For each service listed in parameter `use_blacklist` (see [`use_blacklist`](../man/sympa.conf.5.md#use_blacklist)), the following implicit scenario rule is added at the beginning of the scenario:

``` code
search(blacklist.txt)  smtp,md5,dkim,smime -> reject,quiet
```

The goal is to block messages or other service requests from unwanted users. The blacklist can be defined for the robot or for the list. At the list level, the blacklist is to be managed by list owner or list editor via the web interface.

Hiding scenario files
---------------------

Because Sympa is distributed with many default scenario files, you may want to hide some of them to list owners (to make list administration menus shorter and readable). To hide a scenario file, you should create an empty file with the `:ignore` suffix. Depending on where this file has been created, it will make it be ignored at either a global, robot or list level.

Example:

By creating [``$SYSCONFDIR``](../layout.md#sysconfdir)`/mail.example.org/scenari/send.intranetorprivate:ignore`, the `intranetorprivate` `send` scenario will be hidden (on the web admin interface), at the `mail.example.org` robot level only.
