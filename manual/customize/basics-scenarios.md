Authorization scenarios
=======================

(Work in progress)

rules specifications
--------------------

(Work in progress)
 
Named Filters
-------------

At the moment Named Filters are only used in authorization scenarios. They enable to select a category of people who will be authorized or not to realise some actions.

As a consequence, you can grant privileges in a list to people belonging to an LDAP directory, an SQL database or an flat text file, thanks to an authorization scenario.

Note that the only a subset of variable available in the scenario context are available here (including \[sender\] and \[listname\]).

### LDAP Named Filters Definition

People are selected through an LDAP filter defined in a configuration file. This file must have the extension '.ldap'. It is stored in `/usr/local/sympa-os/etc/search_filters/`.

You must give several informations in order to create a LDAP Named Filter:

  - host
    A list of host:port LDAP directories (replicates) entries.
  - suffix
    Defines the naming space covered by the search (optional, depending on the LDAP server).
  - filter
    Defines the LDAP search filter (RFC 2254 compliant). But you must absolutely take into account the first part of the filter which is: ('mail\_attribute' = \[sender\]) as shown in the example. you will have to replce 'mail\_attribute' by the name of the attribute for the email. *Sympa* verifies if the user belongs to the category of people defined in the filter.
  - scope
    By default the search is performed on the whole tree below the specified base object. This may be changed by specifying a scope :

      - base : Search only the base object.
      - one
        Search the entries immediately below the base object.
      - sub
        Search the whole tree below the base object. This is the default.

  - bind\_dn
    If anonymous bind is not allowed on the LDAP server, a DN and password can be used.
  - bind\_password
    This password is used, combined with the bind\_dn above.

example.ldap : we want to select the professors of mathematics in the university of Rennes1 in France
``` code        
host        ldap.univ-rennes1.fr:389,ldap2.univ-rennes1.fr:390
suffix      dc=univ-rennes1.fr,dc=fr
filter      (&(canonic_mail = [sender])(EmployeeType = prof)(subject = math))
scope       sub
```

### SQL Named Filters Definition

People are selected through an SQL filter defined in a configuration file. This file must have the extension '.sql'. It is stored in `/usr/local/sympa-os/etc/search_filters/`.

To create an SQL Named Filter, you have to configure SQL host, database and options, the same way you did it for the main Sympa database in sympa.conf. Of course you can use different database and options. Sympa will open a new Database connection to execute your statement.

Please refer to section [7.10](node8.html#database-related), page [![\[\*\]](crossref.png)](node8.html#database-related) for a detailed explanation of each parameter.

Here, all database parameters have to be grouped in one `sql_named_filter_query` paragraph.

  - db\_type
    `Format: db_type mysql | SQLite | Pg | Oracle | Sybase` Database management system used. Mandatory and Case sensitive.
  - db\_host
    Database host name. Mandatory.
  - db\_name
    Name of database to query. Mandatory.
  - statement
    Mandatory. The SQL statement to execute to verify authorization. This statement must returns 0 to refuse the action, or anything else to grant privileges. The `SELECT COUNT(*)...` statement is the perfect query for this parameter. The keyword in the SQL query will be replaced by the sender's email.
  - Optional parameters
    Please refer to main sympa.conf section for description.
      - db\_user
      - db\_password
      - db\_options
      - db\_env
      - db\_port
      - db\_timeout

example.sql : we want to select the professors of mathematics in the university of Rennes1 in France
``` code
sql_named_filter_query
db_type         mysql
db_name         people
db_host         dbserver.rennes1.fr
db_user         sympa
db_passwd       pw_sympa_mysqluser
statement       SELECT count(*) as c FROM users WHERE mail=[sender] AND EmployeeType='PROFESSOR' AND department='mathematics'
```

### Search Condition

The search condition is used in authorization scenarios which are defined and described in (see [14](#scenarios))

The syntax of this rule is:
``` code:
search(example.ldap)      smtp,smime,md5    -> do_it
search(blacklist.txt)     smtp,smime,md5    -> do_it
```

The variable used by 'search' is the name of the LDAP Configuration file or a txt matching enumeration

+Note that *Sympa* processes maintain a cache of processed search conditions to limit access to the LDAP directory or SQL server; each entry has a lifetime of 1 hour in the cache.

When using .txt file extention, the file is read looking for a line that match the second parameter (usually the user email address). Each line is a string where the char \* can be used once to mach any block. This feature is used by the blacklist implicit scenario rule. (see [![\[\*\]](crossref.png)](#blacklist))

The method of authentication does not change.

scenario inclusion
------------------

Scenarios can also contain includes :
``` code
subscribe
    include commonreject
    match(, /cru\.fr$/)          smtp,smime -> do_it
true()                               smtp,smime -> owner
```

In this case sympa applies recursively the scenario named `include.commonreject` before introducing the other rules. This possibility was introduced in order to facilitate the administration of common rules.

You can define a set of common scenario rules, used by all lists. include.`<`action`>`.header is automatically added to evaluated scenarios. Note that you will need to restart Sympa processes to force reloading of list config files.

blacklist implicit rule
-----------------------

For each service listed in parameter `use_blacklist` (see [7.4.4](node8.html#useblacklist)), the following implicit scenario rule is added at the beginning of the scenario :
``` code
search(blacklist.txt)  smtp,md5,pgp,smime -> reject,quiet
```

The goal is to block message or other service request from unwanted users. The blacklist can be defined for the robot or for the list. The one at the list level is to managed by list owner or list editor via the web interface.

Custom perl package conditions
------------------------------

You can use a perl package of your own to evaluate a custom condition. It could be usefull if you have very complex tasks to accomplish to evaluate your condition (web services queries...). You write a perl module, place it in the CustomCondition namespace, with one verify fonction that have to return 1 to grant access, undef to throw an error, or anything else to refuse the authorization.

This perl module:

  - must be placed in a subdirectoy `'custom_conditions'` of the `'etc'` directory of your sympa installation, or of a robot
  - its filename must be lowercase
  - must be placed in the CustomCondition namespace
  - must contains one `'verify'` static fonction
  - will receive all condition arguments as parameters

For example, lets write the smallest custom condition that always returns 1.

/home/sympa/etc/custom_conditions/yes.pm :
``` code
#!/usr/bin/perl

package CustomCondition::yes;

use strict;
use Log; # optional : we log parameters

sub verify {
  my @args = @_;
  foreach my $arg (@args) {
    do_log ('debug3', 'arg: %s', $arg);
  }
  # I always say 'yes'
  return 1;
}
## Packages must return true.
1;
```

We can use this custom condition that way :
``` code
CustomCondition::yes(,,)      smtp,smime,md5    -> do_it
true()                               smtp,smime -> reject
```
Note that the `,,` are optionnal, but it's the way you can pass information to your package. Our yes.pm will print their values in the logs.

Remember that the package name has to be small letters, but the 'CustomCondition' namespace is case sensitive. If your package return undef, the sender will receive an 'internal error' mail. If it returns anything else but '1', the sender will receive a 'forbidden' error.

Hidding scenario files
----------------------

Because *Sympa* is distributed with many default scenario files, you may want to hidde some of them to list owners (to make list admin menus shorter and readable). To hidde a scenario file you should create an empty file with the `:ignore` suffix. Depending on where this file has been created will make it ignored at either a global, robot or list level.

*Example :*
``` code
> /usr/local/sympa-os/etc/my.domain.org/scenari/send.intranetorprivate:ignore
```
The `intranetorprivate` `send` scenario will be hidden (on the web admin interface), at the my.domain.orgrobot level only.
