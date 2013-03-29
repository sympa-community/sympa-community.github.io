Automatic list creation
-----------------------

----
Note:

  * **Situation**: you want to create lists according to specified criteria (age, geographical location, etc.).
*
    *Problem**: Creating all the possible lists would result in thousands of list creation, many of them (but you don't know which one...) would never be used.

----

Automatic list creation allows you to define those potential lists through family parameters, but they will not be created at once. The mailing list creation is triggered when Sympa receives a message addressed to this list.

To enable automatic list creation, you will have to:

  - configure your MTA to queue messages for these lists in an appropriate spool;
  - define a family associated to such lists;
  - configure Sympa to enable the feature.

### Configuring your MTA

#### The familyqueue solution (with postfix)

To do so, you have to configure your MTA for it to add a custom header field to messages. The easiest way is to customize your aliases manager, so that mails for automatic lists are not delivered to the normal `queue` program, but to the `familyqueue` dedicated one. For example, you can decide that the name of those lists will start with the `auto-` pattern, so you can process them separately from other lists you are hosting.

`familyqueue` expects 2 arguments: the list name and family name (whereas the `queue` program only expects the list address).

Now let's start with a use case: we need to communicate to groups of co-workers, depending on their age and their occupation. We decide that, for example, if we need to write to all CTOs who are fifty years old, we will use the `auto-cto.50@lists.domain.com` mailing list. The occupation and age informations are stored in our LDAP directory (but of course we could use any Sympa data source: SQL, files...). We will create the `age-occupation` family.

First of all we configure our MTA to deliver mail to `'auto-*`' to `familyqueue` for the `age-occupation` family. We'll also need to tell the MTA to accept mail for addresses that do not yet exist since by default postfix will reject mail for unknown local users.

/etc/postfix/main.cf
``` code
...
transport_maps = regexp:/etc/postfix/transport_regexp
local_recipient_maps = pcre:/etc/postfix/local_recipient_regexp unix:passwd.byname $alias_maps
```
/etc/postfix/transport_regexp
``` code
/^.*-owner\@lists\.domain\.com$/      sympabounce:
/^auto-.*\@lists\.domain\.com$/       sympafamily:
/^.*\@lists\.domain\.com$/            sympa:
```
/etc/postfix/local_recipient_regexp
``` code
/^.*-owner\@lists\.domain\.com$/  1
/^auto-.*\@lists\.domain\.com$/   1
```
/etc/postfix/master.cf
``` code
sympa     unix  -       n       n       -       -       pipe
  flags=R user=sympa argv=/home/sympa/bin/queue ${recipient}
sympabounce  unix  -       n       n       -       -       pipe
  flags=R user=sympa argv=/home/sympa/bin/bouncequeue ${user}
sympafamily  unix  -       n       n       -       -       pipe
  flags=R user=sympa argv=/home/sympa/bin/familyqueue ${user} age-occupation
```

A mail sent to `auto-cto.50@lists.domain.com` will be queued to the `/home/sympa/spool/automatic` spool, defined by the `queueautomatic` `sympa.conf` parameter (see [queueautomatic](/conf-parameters/part2#queueautomatic)). The mail will first be processed by an instance of the `sympa.pl` process dedicated to automatic list creation, then the mail will be sent to the newly created mailing list.

#### The sympa-milter solution (with sendmail)

If you don't use postfix or don't want to dig in postfix alias management, you have an alternative solution for automatic listes management: sympa-milter.

This program is a contribution by [Jose-Marcio Martins da Cruz](mailto:Jose-Marcio.Martins@ensmp.fr).

What it does is checking all incoming mails and, if it recognizes a message to an automatic list, adds the relevant headers in it and places it in Sympa's automatic spool. It replaces familyqueue.

For all the doc, we assume you're using sendmail.

This is the procedure to make it work:

##### Install sympa-milter

You can download the latest version at the following address: [http://j-chkmail.ensmp.fr/sympa-milter/](http://j-chkmail.ensmp.fr/sympa-milter/).

Once you have the archive, decompress it: `tar xzvf sympa-milter-0.6.tgz`.

You will need the `libmilter` library to build the sympa milter. This can be found in the development files of sendmail. You can install it, for example on a RedHat system, with the following command:

``` code
yum install sendmail-devel
```

Then install the program:

``` code
# cd sympa-milter-0.6/
# ./configure
# make
# make install
```

The default install directory is `/usr/local/sympa-milter/` (you can change this value with the `–prefix` configure option).

The install process also adds a launcher into `/etc/init.d/`, named `sympa-milter`. You'll need to setup links to it under `/etc/rc3.d`. If you're using Fedora like Linux distributions, you can use `/sbin/chkconfig` to setup these links.

``` bash
/sbin/chkconfig sympa-milter on
```

You must then set up the configuration file, `sympa-milter.conf`. You will find a sample configuration file inside `/usr/local/sympa-milter/etc` directory. This file contains two sections whose border are XML-like tags. Inside a section, a parameter is defined on a single line by the sequence:

`parameters_name            parameter_value`

  - the general section, between the `<general>` and `</general>` tags is used to define, well general parameters, related to the program execution. It contains the following items:
      - log\_level (positive integer value): the amount of logs generated by sympa-milter;
      - log\_facility (string): the syslog facility in which the program will log;
      - log\_severity (string: yes/no): If you enable this, `syslog` will include a string like `[ID 000000 local6.info]` in each log line, allowing you to identify the log level and facility.
      - socket (string): the socket used by the application; must be the same as the one defined in your MTA;
      - spool\_dir (string): the absolute path to the`automatic` spool in which messages should be placed;
      - pid\_file (string): the absolute path to the pid file (default = `/usr/local/sympa-milter/var/sympa-milter.pid`);
      - run\_as\_user (string) the user the uid under which to execute sympa-milter (default = `sympa`, but changeable by a `configure` script option); this must be the same as the one running sympa;
      - run\_as\_group the group the gid under which to execute sympa-milter (default = `sympa`, but changeable by a `configure` script option); this must be the same as the one running sympa;
  - the family definition section, between the `<families>` and `</families>` tags is used to define the regular expressions which will allow sympa-milter to catch list creation messages. This section can contain an unlimited number of identically built lines, following this syntax:

``` code
family        recipient_regular_expression
```

You should use "plussed aliases" (at least with sendmail) to identify user existence more easily.

Here is an example of `sympa-milter.conf`, filled-up with default values :

``` code
#
# Section general
#
<general>
log_level        10
log_facility            local6
log_severity            yes

socket                  inet:2030@localhost

spool_dir               /usr/local/sympa-milter/var

pid_file                /usr/local/sympa-milter/var/sympa-milter.pid

run_as_user             sympa
run_as_group            sympa
</general>
#
# Section families
#
<families>
# Syntax :
#     family        recipient regular expression
#
joe                  ^joe+.*@one.domain.com
toto                 ^bob+toto@other.domain.com
best                 ^best.*@another.domain.com
</families>
```

----
**Note:**

  * It is probably better to make all your regular expression start with "^". This way, bouncing messages won't be caught by sympa-milter and normally processed.

----

You can use any regular expression to define the addresses used by your family.

##### Set up your MTA

What you must do to make all the thingy to work is:

  - setting up your MTA to use sympa-milter:

``` code
O InputMailFilters=sympa-milter
Xsympa-milter, S=inet:2030@localhost, T=C:2m;S:20s;R:20s;E:5m
```

  - defining aliases to prevent sendmail from howling that a user (corresponding to your automatic list) doesn't exist. If all your automatic lists start with "auto", for example you can write:

``` code
auto    : /dev/null
```

**or**

``` code
auto    : "some_file"
```

Reload your MTA config. All set!

### Defining the list family

We need to create the appropriate `etc/families/age-occupation/config.tt2`. All the magic comes from the TT2 language capabilities. We define on-the-fly the LDAP source, thanks to TT2 macros.

/home/sympa/etc/families/age-occupation/config.tt2
``` code
...
user_data_source include2

[%
occupations = {
    cto = { title=>"chief technical officer", abbr=>"CHIEF TECH OFF" },
    coo = { title=>"chief operating officer", abbr=>"CHIEF OPER OFF" },
    cio = { title=>"chief information officer", abbr=>"CHIEF INFO OFF" },
}
nemes = listname.split('-');
THROW autofamily "SYNTAX ERROR: listname must begin with 'auto-' " IF (nemes.size != 2 || nemes.0 != 'auto');
tokens = nemes.1.split('\.');
THROW autofamily "SYNTAX ERROR: wrong listname syntax" IF (tokens.size != 2 || ! occupations.${tokens.0} || tokens.1 < 20 || tokens.1 > 99 );
age = tokens.1 div 10;
%]

custom_subject [[% occupations.${tokens.0}.abbr %] OF [% tokens.1 %]]

subject Every [% tokens.1 %] years old [% occupations.${tokens.0}.title %]

include_ldap_query
attrs mail
filter (&(objectClass=inetOrgPerson)(employeeType=[% occupations.${tokens.0}.abbr %])(personAge=[% age %]*))
name ldap
port 389
host ldap.domain.com
passwd ldap_passwd
suffix dc=domain,dc=com
timeout 30
user cn=root,dc=domain,dc=com
scope sub
select all
```

The main variable you get is the name of the current mailing list via the `listname` variable as used in the example above.

### Configuring Sympa

Now we need to enable automatic list creation in Sympa. To do so, we have to:

  - set the `automatic_list_feature` parameter to `on` and define who can create automatic lists via the `automatic_list_creation` (points to an automatic\_list\_creation scenario);
  - set the `queueautomatic` `sympa.conf` parameter to the spool location where we want these messages to be stored (it has to be different from the `/home/sympa/spool/msg` spool).

You can make Sympa delete automatic lists that were created with zero list members; to do so, you should set the `automatic_list_removal` parameter to `if_empty`.

/home/sympa/etc/sympa.conf
``` code
...
automatic_list_feature  on
automatic_list_creation public
queueautomatic          /home/sympa/spool/automatic
automatic_list_removal    if_empty
```

While writing your own `automatic_list_creation` scenarios, be aware that:

  - when the scenario is evaluated, the list is not yet created; therefore you can not use the list-related variables;
  - you can only use the `smtp` and `smime` authentication methods in scenario rules (you cannot request the md5 challenge). Moreover, only the `do_it` and `reject` actions are available.

Now you can send message to auto-cio.40 or auto-cto.50, and the lists will be created on the fly.

You will receive an 'unknown list' error if either the syntax is incorrect or the number of subscriber is zero.

