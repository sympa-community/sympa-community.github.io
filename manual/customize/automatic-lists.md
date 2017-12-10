Automatic list creation
-----------------------

You can benefit from the family concept to let Sympa automatically create lists for you. Let us suppose that you want to open a list according to specified criteria (age, geographical site...) within your organization. Maybe that would result in too many lists, and many of them would never be used.

Automatic list creation allows you to define those potential lists through family parameters, but they won't be created yet. The mailing list creation is trigerred when Sympa receives a message addressed to this list.

To enable automatic list creation you'll have to :

  - Configure your MTA to queue messages for these lists in an appropriate spool
  - Define a family associated to such lists
  - Configure Sympa to enable the feature

### Configuring your MTA

To do so, we have to configure our MTA for it to add a custom header field to the message. The easiest way is to customize your aliases manager, so mails for automatic lists aren't delivered to the normal `queue` program, but to the `familyqueue` dedicated one. For example, you can decide that the name of those lists will start with the `auto-` pattern, so you can process them separately from other lists you are hosting.

`familyqueue` expects 2 arguments : the list name and the family name (whereas the `queue` program only expects the list address).

Let's start with a use case : we need to communicate to groups of co-workers, depending on their age and their occupation. We decide that, for example, if I need to write to all CTOs who are fifty years old, I will use the auto-cto.50@lists.domain.com mailing list. The occupation and age informations are stored in our ldap directory (but of course we could use any Sympa data source : sql, files...). We will create the age-occupation family.

First of all we configure our MTA to deliver mail to `'auto-*'` to `familyqueue` for the `age-occupation` family.

/etc/postfix/main.cf
``` code
...
transport_maps = regexp:/etc/postfix/transport_regexp
```
/etc/postfix/transport_regexp
```
/^.*+owner\@lists\.domain\.com$/      sympabounce:
/^auto-.*\@lists\.domain\.com$/       sympafamily:
/^.*\@lists\.domain\.com$/            sympa:
```
/etc/postfix/master.cf
```
sympa     unix  -       n       n       -       -       pipe
  flags=R user=sympa argv=/usr/local/sympa-os/bin/queue ${recipient}
sympabounce  unix  -       n       n       -       -       pipe
  flags=R user=sympa argv=/usr/local/sympa-os/bin/bouncequeue ${user}
sympafamily  unix  -       n       n       -       -       pipe
  flags=R user=sympa argv=/usr/local/sympa-os/bin/familyqueue ${user} age-occupation
```

A mail addressed to *auto-cto.50@lists.domain.com* will be queued to the `/usr/local/sympa-os/spool/automatic` spool, defined by the `queueautomatic` `sympa.conf` parameter (see [7.6.11](node8.html#kw-queueautomatic), page [![\[\*\]](crossref.png)](node8.html#kw-queueautomatic)). The mail will first be processed by an instance of `sympa.pl` process dedicated to automatic list creation, then the mail will be sent to the newly created mailing list.

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
THROW autofamily "SYNTAX ERROR : listname must begin with 'auto-' " IF (nemes.size != 2 || nemes.0 != 'auto');
tokens = nemes.1.split('\.');
THROW autofamily "SYNTAX ERROR : wrong listname syntax" IF (tokens.size != 2 || ! occupations.${tokens.0} || tokens.1 < 20 || tokens.1 > 99 );
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
The main variable you get is the name of the current mailing list via the **listname** variable as used in the example above.

### Configuring Sympa

Now we need to enable automatic list creation in Sympa. To do so, we have to

  - set the `automatic_list_feature` parameter to `on` and define who can create automatic lists via the `automatic_list_creation` (points to an automatic\_list\_creation scenario).
  - set the `queueautomatic` `sympa.conf` parameter to the spool location where we want these messages to be stored (it has to be different from the `/usr/local/sympa-os/spool/msg` spool).

You can make Sympa delete automatic lists that were created with zero list members ; to do so you shoukd set the `automatic_list_removal` parameter to `if_empty`.

/home/sympa/etc/sympa.conf
``` code
...
automatic_list_feature  on
automatic_list_creation public
queueautomatic          /usr/local/sympa-os/spool/automatic
automatic_list_removal    if_empty
```

While writing your own automatic\_list\_creation scenarios, be aware that :

  - when the scenario is evaluated, the list is not yet created ; therefore you can't use the list-related variables.
  - You can only use 'smtp' and 'smime' authentication method in scenario rules (You cannot request the md5 challenge). Moreover only `do_it` and `reject` actions are available.

Now you can send message to auto-cio.40 or auto-cto.50, and the lists will be created on the fly.

You will receive an 'unknown list' error if either the syntax is incorrect or the number of subscriber is zero.

