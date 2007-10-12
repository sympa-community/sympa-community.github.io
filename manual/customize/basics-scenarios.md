Authorization scenarios
=======================

(Work in progress)

Location of scenario file
-------------------------

(Work in progress)

When customizing scenario for your own site, robot or list, don't modify .../sympa/bin/scenari content or next Sympa update will overwrite it (you must never modify anything in .../sympa/bin/ unless your are patching Sympa). You can modify Sympa behavior if you create new scenario which name is the same as one of the scenario included in the distribution but with a location related to target site, robot or list. You can also add a new scenario ; it will automatically add an accepted value for the related parameter.

----
Note:

  * When modifying a existing scenario you need to restart Sympa or touch list config file before Sympa use it.

----

Scenario structure
------------------

Basically, a scenario file is composed of a title on the first line and a set of rules on the following lines.

### Scenario title

The first line of a scenario file can contain its title. This is the text that will later appear in the drop-down menu of your administration web interface. This title can be just plain text:

``` code
Restricted to subscribers
```

It can also be set to be internationalized:

``` code
title.gettext Restricted to subscribers
```

That way, the character string following `title.gettext` can be handled by Sympa internationalization process.

### Rules overview

(Work in progress)

### Rules specifications

(Work in progress)

The function to evaluate scenario is described in section [internals](/internals).

`custom_vars` allows you to introduce [custom parameters](/manual/customizing#custom_parameters) in your scenario.

(Work in progress)

Named Filters
-------------

At the moment, Named Filters are only used in authorization scenarios. They enable to select a category of people who will be authorized or not to realize some actions.

As a consequence, you can grant privileges in a list to people belonging to an LDAP directory, an SQL database or a flat text file, thanks to an authorization scenario.

Note that only a subset of variables available in the scenario context are available here (including \[sender\] and \[listname\]).

### LDAP Named Filters Definition

People are selected through an LDAP filter defined in a configuration file. This file must have the extension '.ldap'. It is stored in `/home/sympa/etc/search_filters/`.

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

People are selected through an SQL filter defined in a configuration file. This file must have the extension '.sql'. It is stored in `/home/sympa/etc/search_filters/`.

To create an SQL Named Filter, you have to configure SQL host, database and options, the same way you did it for the main Sympa database in `sympa.conf`. Of course, you can use different database and options. Sympa will open a new Database connection to execute your statement.

Please refer to section [Database related](/conf-parameters/part3#database_related) for a detailed explanation of each parameter.

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

When using the '.txt' file extension, the file is read looking for a line that matches the second parameter (usually the user email address). Each line is a string where the char `*` can be used once to mach any block. This feature is used by the blacklist implicit scenario rule (see [Blacklist](/conf-parameters/part2#use_blacklist)).

The method of authentication does not change.

Scenario inclusion
------------------

Scenarios can also contain includes:

``` code
subscribe
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

For each service listed in parameter `use_blacklist` (see [use_blacklist](/manual/conf-parameters/part2#use_blacklist)), the following implicit scenario rule is added at the beginning of the scenario:

``` code
search(blacklist.txt)  smtp,md5,pgp,smime -> reject,quiet
```

The goal is to block messages or other service requests from unwanted users. The blacklist can be defined for the robot or for the list. At the list level, the blacklist is to be managed by list owner or list editor via the web interface.

Custom Perl package conditions
------------------------------

You can use a Perl package of your own to evaluate a custom condition. It can be useful if you have very complex tasks to carry out to evaluate your condition (web services queries...). In this case, you should write a Perl module, place it in the `CustomCondition` namespace, with one verify fonction that has to return `1` to grant access, `undef` to warn of an error, or anything else to refuse the authorization.

This Perl module:

  - must be placed in a subdirectoy `custom_conditions` of the `etc` directory of your Sympa installation, or of a robot;

  - its filename must be lowercase;

  - must be placed in the `CustomCondition` namespace;

  - must contain one `verify` static fonction;

  - will receive all condition arguments as parameters.

For example, lets write the smallest custom condition that always returns `1`.

/home/sympa/etc/custom_conditions/yes.pm :
``` code
#!/usr/bin/perl

package CustomCondition::yes;

use strict;
use Log; # optional : we log parameters

sub verify {
  my @args = @_;
  foreach my $arg (@args) {
    do_log ('debug3', 'arg: ', $arg);
  }
  # I always say 'yes'
  return 1;
}
## Packages must return true.
1;
```

We can use this custom condition that way:

``` code
CustomCondition::yes(,,)      smtp,smime,md5    -> do_it
true()                               smtp,smime -> reject
```

Note that the `,,` are optional, but it is the way you can pass information to your package. Our `yes.pm` will print the values in the logs.

Remember that the package name has to be lowercase, but the `CustomCondition` namespace is case sensitive. If your package returns `undef`, the sender will receive an 'internal error' mail. If it returns anything else but `1`, the sender will receive a 'forbidden' error.

Hiding scenario files
---------------------

Because Sympa is distributed with many default scenario files, you may want to hide some of them to list owners (to make list administration menus shorter and readable). To hide a scenario file, you should create an empty file with the `:ignore` suffix. Depending on where this file has been created, it will make it be ignored at either a global, robot or list level.

Example:

``` code
/home/sympa/etc/my.domain.org/scenari/send.intranetorprivate:ignore
```

The `intranetorprivate` `send` scenario will be hidden (on the web admin interface), at the my.domain.org robot level only.
