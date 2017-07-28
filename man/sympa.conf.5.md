# NAME

sympa.conf, robot.conf - Configuration file for default site and robot

# DESCRIPTION

`/etc/sympa/sympa.conf` is main configuration file of Sympa.
Several parameters definied in this file may be overridden by `robot.conf`
configuration file for each robot.

Format of `sympa.conf` and `robot.conf` is as following:

- Lines beginning with `#` and containing only spaces are ignored.
- Each line has the form "_parameter_ _value_".
_value_ may contain spaces but may not contain newlines.

Below is entire list of configuration parameters.
For more details of each parameter see Reference Manual.

## Parameters

### Site customization

- domain

    (Mandatory)
    (Overridable by each `robot.conf`)

    Main robot hostname

    Example:

        domain domain.tld

- host

    (Optioal)
    (Overridable by each `robot.conf`)

- email

    (Default value: `sympa`)
    (Overridable by each `robot.conf`)

    Local part of sympa email address

    Effective address will be \[EMAIL\]@\[HOST\]

- gecos

    (Default value: `SYMPA`)
    (Overridable by each `robot.conf`)

    Gecos for service mail sent by Sympa itself

    This parameter is used for display name in the "From:" header

- listmaster

    (Mandatory)
    (Overridable by each `robot.conf`)

    Listmasters email list comma separated

    Example:

        listmaster your_email_address@domain.tld

    Sympa will associate listmaster privileges to these email addresses (mail and web interfaces). Some error reports may also be sent to these addresses.

- listmaster\_email

    (Default value: `listmaster`)
    (Overridable by each `robot.conf`)

    Local part of listmaster email address

- wwsympa\_url

    (Mandatory)
    (Overridable by each `robot.conf`)

    URL of main Web page

    Example:

        wwsympa_url http://host.domain.tld/sympa

- soap\_url

    (Optioal)
    (Overridable by each `robot.conf`)

- process\_archive

    (Default value: `off`)
    (Overridable by each `robot.conf`)

    Store distributed messages into archive

    This setting can be overridden by each list

- voot\_feature

    (Default value: `off`)

- max\_wrong\_password

    (Default value: `19`)
    (Overridable by each `robot.conf`)

- spam\_protection

    (Default value: `javascript`)
    (Overridable by each `robot.conf`)

- web\_archive\_spam\_protection

    (Default value: `cookie`)
    (Overridable by each `robot.conf`)

- color\_0

    (Default value: `#F7F7F7`)
    (Overridable by each `robot.conf`)

- color\_1

    (Default value: `#222222`)
    (Overridable by each `robot.conf`)

- color\_2

    (Default value: `#004B94`)
    (Overridable by each `robot.conf`)

- color\_3

    (Default value: `#5E5E5E`)
    (Overridable by each `robot.conf`)

- color\_4

    (Default value: `#4c4c4c`)
    (Overridable by each `robot.conf`)

- color\_5

    (Default value: `#0090E9`)
    (Overridable by each `robot.conf`)

- color\_6

    (Default value: `#005ab2`)
    (Overridable by each `robot.conf`)

- color\_7

    (Default value: `#fff`)
    (Overridable by each `robot.conf`)

- color\_8

    (Default value: `#f2f6f9`)
    (Overridable by each `robot.conf`)

- color\_9

    (Default value: `#bfd2e1`)
    (Overridable by each `robot.conf`)

- color\_10

    (Default value: `#983222`)
    (Overridable by each `robot.conf`)

- color\_11

    (Default value: `#66aaff`)
    (Overridable by each `robot.conf`)

- color\_12

    (Default value: `#FFE7E7`)
    (Overridable by each `robot.conf`)

- color\_13

    (Default value: `#f48A7b`)
    (Overridable by each `robot.conf`)

- color\_14

    (Default value: `#ff9`)
    (Overridable by each `robot.conf`)

- color\_15

    (Default value: `#fe57a1`)
    (Overridable by each `robot.conf`)

- dark\_color

    (Default value: `silver`)
    (Overridable by each `robot.conf`)

- light\_color

    (Default value: `#aaddff`)
    (Overridable by each `robot.conf`)

- text\_color

    (Default value: `#000000`)
    (Overridable by each `robot.conf`)

- bg\_color

    (Default value: `#ffffcc`)
    (Overridable by each `robot.conf`)

- error\_color

    (Default value: `#ff6666`)
    (Overridable by each `robot.conf`)

- selected\_color

    (Default value: `silver`)
    (Overridable by each `robot.conf`)

- shaded\_color

    (Default value: `#66cccc`)
    (Overridable by each `robot.conf`)

- logo\_html\_definition

    (Optioal)
    (Overridable by each `robot.conf`)

- favicon\_url

    (Optioal)
    (Overridable by each `robot.conf`)

- main\_menu\_custom\_button\_1\_title

    (Optioal)
    (Overridable by each `robot.conf`)

- main\_menu\_custom\_button\_1\_url

    (Optioal)
    (Overridable by each `robot.conf`)

- main\_menu\_custom\_button\_1\_target

    (Optioal)
    (Overridable by each `robot.conf`)

- main\_menu\_custom\_button\_2\_title

    (Optioal)
    (Overridable by each `robot.conf`)

- main\_menu\_custom\_button\_2\_url

    (Optioal)
    (Overridable by each `robot.conf`)

- main\_menu\_custom\_button\_2\_target

    (Optioal)
    (Overridable by each `robot.conf`)

- main\_menu\_custom\_button\_3\_title

    (Optioal)
    (Overridable by each `robot.conf`)

- main\_menu\_custom\_button\_3\_url

    (Optioal)
    (Overridable by each `robot.conf`)

- main\_menu\_custom\_button\_3\_target

    (Optioal)
    (Overridable by each `robot.conf`)

- css\_path

    (Optioal)
    (Overridable by each `robot.conf`)

- css\_url

    (Optioal)
    (Overridable by each `robot.conf`)

- static\_content\_path

    (Default value: `/home/sympa/static_content`)
    (Overridable by each `robot.conf`)

    Directory for storing static contents (CSS, members pictures, documentation) directly delivered by HTTP server

- static\_content\_url

    (Default value: `/static-sympa`)
    (Overridable by each `robot.conf`)

    URL mapped with the static\_content\_path directory defined above

- pictures\_feature

    (Default value: `on`)

- pictures\_max\_size

    (Default value: `102400`)
    (Overridable by each `robot.conf`)

- cookie

    (Optioal)

    Secret used by Sympa to make MD5 fingerprint in web cookies secure

    Example:

        cookie 123456789

    Should not be changed ! May invalid all user password

- create\_list

    (Default value: `public_listmaster`)
    (Overridable by each `robot.conf`)

    Value of this parameter is name of `create_list` scenario.

    Who is able to create lists

    This parameter is a scenario, check sympa documentation about scenarios if you want to define one

- global\_remind

    (Default value: `listmaster`)

    Value of this parameter is name of `global_remind` scenario.

- move\_user

    (Default value: `auth`)
    (Overridable by each `robot.conf`)

    Value of this parameter is name of `move_user` scenario.

    Who is able to change user's email

- allow\_subscribe\_if\_pending

    (Default value: `on`)
    (Overridable by each `robot.conf`)

- custom\_robot\_parameter

    (Optioal)
    (Overridable by each `robot.conf`)

    Used to define a custom parameter for your server. Do not forget the semicolon between the param name and the param value.

### Directories

- home

    (Default value: `/home/sympa/list_data`)

    Directory containing mailing lists subdirectories

- etc

    (Default value: `/home/sympa/etc`)

    Directory for configuration files; it also contains scenari/ and templates/ directories

### System related

- syslog

    (Default value: `LOCAL1`)

    Syslog facility for sympa

    Do not forget to edit syslog.conf

- log\_level

    (Default value: `0`)
    (Overridable by each `robot.conf`)

    Log verbosity

    0: normal, 2,3,4: for debug

- log\_socket\_type

    (Default value: `unix`)

    Communication mode with syslogd (unix | inet)

- log\_condition

    (Optioal)
    (Overridable by each `robot.conf`)

- log\_module

    (Optioal)
    (Overridable by each `robot.conf`)

- umask

    (Default value: `027`)

    Umask used for file creation by Sympa

### Sending related

- sendmail

    (Default value: `/usr/sbin/sendmail`)

    Path to the MTA (sendmail, postfix, exim or qmail)

    should point to a sendmail-compatible binary (eg: a binary named "sendmail" is distributed with Postfix)

- sendmail\_args

    (Default value: `-oi -odi -oem`)

- maxsmtp

    (Default value: `40`)

    Max. number of Sendmail processes (launched by Sympa) running simultaneously

    Proposed value is quite low, you can rise it up to 100, 200 or even 300 with powerful systems.

- merge\_feature

    (Default value: `off`)

- automatic\_list\_removal

    (Default value: `none`)
    (Overridable by each `robot.conf`)

- automatic\_list\_feature

    (Default value: `off`)
    (Overridable by each `robot.conf`)

- automatic\_list\_creation

    (Default value: `public`)
    (Overridable by each `robot.conf`)

    Value of this parameter is name of `automatic_list_creation` scenario.

- automatic\_list\_families

    (Optioal)
    (Overridable by each `robot.conf`)

    Defines the name of the family the automatic lists are based on.

- automatic\_list\_prefix

    (Optioal)

    Defines the prefix allowing to recognize that a list is an automatic list.

- log\_smtp

    (Default value: `off`)
    (Overridable by each `robot.conf`)

- use\_blacklist

    (Default value: `send,create_list`)
    (Overridable by each `robot.conf`)

    comma separated list of operations for which blacklist filter is applied

    Setting this parameter to "none" will hide the blacklist feature

- reporting\_spam\_script\_path

    (Optioal)
    (Overridable by each `robot.conf`)

    If set, when a list editor report a spam, this external script is run by wwsympa or sympa, the spam is sent into script stdin

- max\_size

    (Default value: `5242880`)
    (Overridable by each `robot.conf`)

    Default maximum size (in bytes) for messages (can be re-defined for each list)

- misaddressed\_commands

    (Default value: `reject`)

- misaddressed\_commands\_regexp

    (Default value: `((subscribe\s+(\S+)|unsubscribe\s+(\S+)|signoff\s+(\S+)|set\s+(\S+)\s+(mail|nomail|digest))\s*)`)

- nrcpt

    (Default value: `25`)

    Maximum number of recipients per call to Sendmail. The nrcpt\_by\_domain.conf file allows a different tuning per destination domain.

- avg

    (Default value: `10`)

    Max. number of different domains per call to Sendmail

- alias\_manager

    (Default value: `/home/sympa/bin/alias_manager.pl`)

- db\_list\_cache

    (Default value: `off`)

    Whether or not to cache lists in the database

- sendmail\_aliases

    (Default value: `/etc/mail/sympa_aliases`)
    (Overridable by each `robot.conf`)

    Path of the file that contains all list related aliases

- aliases\_program

    (Default value: `newaliases`)
    (Overridable by each `robot.conf`)

    Program used to update alias database.  "makemap", "newaliases", "postalias", "postmap" or full path to custom program

- aliases\_db\_type

    (Default value: `hash`)
    (Overridable by each `robot.conf`)

    Type of alias database.  "btree", "dbm", "hash" and so on.  Available when aliases\_program is "makemap", "postalias" or "postmap"

- rfc2369\_header\_fields

    (Default value: `help,subscribe,unsubscribe,post,owner,archive`)

    Specify which rfc2369 mailing list headers to add

- remove\_headers

    (Default value: `X-Sympa-To,X-Family-To,Return-Receipt-To,Precedence,X-Sequence,Disposition-Notification-To,Sender`)

    Specify header fields to be removed before message distribution

- remove\_outgoing\_headers

    (Default value: `none`)

- reject\_mail\_from\_automates\_feature

    (Default value: `on`)

    Reject mail from automates (crontab, etc) sent to a list?

- ignore\_x\_no\_archive\_header\_feature

    (Default value: `off`)

- anonymous\_header\_fields

    (Default value: `Authentication-Results,Disposition-Notification-To,DKIM-Signature,Injection-Info,Organisation,Organization,Original-Recipient,Originator,Path,Received,Received-SPF,Reply-To,Resent-Reply-To,Return-Receipt-To,X-Envelope-From,X-Envelope-To,X-Sender,X-X-Sender`)

- list\_check\_smtp

    (Optioal)
    (Overridable by each `robot.conf`)

    SMTP server to which Sympa verify if alias with the same name as the list to be created

    Default value is real FQDN of host. Set \[HOST\]:\[PORT\] to specify non-standard port.

- list\_check\_suffixes

    (Default value: `request,owner,editor,unsubscribe,subscribe`)
    (Overridable by each `robot.conf`)

- list\_check\_helo

    (Optioal)
    (Overridable by each `robot.conf`)

    SMTP HELO (EHLO) parameter used for alias verification

    Default value is the host part of list\_check\_smtp parameter.

- urlize\_min\_size

    (Default value: `10240`)
    (Overridable by each `robot.conf`)

- sender\_headers

    (Default value: `From`)

    Header field name(s) used to determine sender of the messages

    Example:

        sender_headers Resent-From,From,Return-Path

    "Return-Path" means envelope sender (a.k.a. "UNIX From") which will be alternative to sender of messages without "From" field.  "Resent-From" may also be inserted before "From", because some mailers add it into redirected messages and keep original "From" field intact.  In particular cases, "Return-Path" can not give right sender: several mail gateway products rewrite envelope sender and add original one as non-standard field such as "X-Envelope-From".  If that is the case, you might want to insert it in place of "Return-Path".

### Bulk mailer

- sympa\_packet\_priority

    (Default value: `5`)
    (Overridable by each `robot.conf`)

    Default priority for a packet to be sent by bulk.

- bulk\_fork\_threshold

    (Default value: `1`)

    Minimum number of packets in database before the bulk forks to increase sending rate

- bulk\_max\_count

    (Default value: `3`)

    Max number of bulks that will run on the same server

- bulk\_lazytime

    (Default value: `600`)

    The number of seconds a slave bulk will remain running without processing a message before it spontaneously dies.

- bulk\_sleep

    (Default value: `1`)

    The number of seconds a bulk sleeps between starting a new loop if it didn't find a message to send.

    Keep it small if you want your server to be reactive.

- bulk\_wait\_to\_fork

    (Default value: `10`)

    Number of seconds a master bulk waits between two packets number checks.

    Keep it small if you expect brutal increases in the message sending load.

### Quotas

- default\_max\_list\_members

    (Default value: `0`)
    (Overridable by each `robot.conf`)

    Default limit for the number of subscribers per list (0 means no limit)

- default\_shared\_quota

    (Optioal)
    (Overridable by each `robot.conf`)

    Default disk quota for shared repository

- default\_archive\_quota

    (Optioal)

### Spool related

- spool

    (Default value: `/home/sympa/spool`)

    Directory containing various specialized spools

    All spool are created at runtime by sympa.pl

- queue

    (Default value: `/home/sympa/spool/msg`)

    Directory for message incoming spool

- queuemod

    (Default value: `/home/sympa/spool/moderation`)

    Directory for moderation spool

- queuedigest

    (Default value: `/home/sympa/spool/digest`)

    Directory for digest spool

- queueauth

    (Default value: `/home/sympa/spool/auth`)

    Directory for authentication spool

- queueoutgoing

    (Default value: `/home/sympa/spool/outgoing`)

    Directory for archive spool

- queuesubscribe

    (Default value: `/home/sympa/spool/subscribe`)

    Directory for subscription spool

- queuetopic

    (Default value: `/home/sympa/spool/topic`)

    Directory for topic spool

- queuebounce

    (Default value: `/home/sympa/spool/bounce`)

    Directory for bounce incoming spool

- queuetask

    (Default value: `/home/sympa/spool/task`)

    Directory for task spool

- queueautomatic

    (Default value: `/home/sympa/spool/automatic`)

    Directory for automatic list creation spool

- queuebulk

    (Default value: `/home/sympa/spool/bulk`)

    Directory for message outgoing spool

- sleep

    (Default value: `5`)

    Must not be 0.

- tmpdir

    (Default value: `/home/sympa/spool/tmp`)

    Temporary directory used by antivirus plugins, MHonArc etc.

- viewmail\_dir

    (Default value: `/home/sympa/spool/viewmail`)

    Directory containing HTML file generated by MHonArc while displaying messages other than archives

- clean\_delay\_queue

    (Default value: `7`)

- clean\_delay\_queueoutgoing

    (Default value: `7`)

- clean\_delay\_queuebounce

    (Default value: `7`)

- clean\_delay\_queuemod

    (Default value: `30`)

- clean\_delay\_queueauth

    (Default value: `30`)

- clean\_delay\_queuesubscribe

    (Default value: `30`)

- clean\_delay\_queuetopic

    (Default value: `30`)

- clean\_delay\_queueautomatic

    (Default value: `10`)

- clean\_delay\_queuebulk

    (Default value: `7`)

- clean\_delay\_queuedigest

    (Default value: `14`)

- clean\_delay\_tmpdir

    (Default value: `7`)

### Internationalization related

- supported\_lang

    (Default value: `ca,cs,de,el,en-US,es,et,eu,fi,fr,gl,hu,it,ja,ko,nb,nl,oc,pl,pt-BR,ru,sv,tr,vi,zh-CN,zh-TW`)
    (Overridable by each `robot.conf`)

    Supported languages

    This is the set of language that will be proposed to your users for the Sympa GUI. Don't select a language if you don't have the proper locale packages installed.

- lang

    (Default value: `en-US`)
    (Overridable by each `robot.conf`)

    Default language (one of supported languages)

    This is the default language used by Sympa

- legacy\_character\_support\_feature

    (Default value: `off`)

    If set to "on", enables support of legacy character set

    See also charset.conf(5) manpage

- filesystem\_encoding

    (Default value: `utf-8`)

### Bounce related

- verp\_rate

    (Default value: `0%`)
    (Overridable by each `robot.conf`)

- welcome\_return\_path

    (Default value: `owner`)

    Welcome message return-path ( unique | owner )

    If set to unique, new subcriber is removed if welcome message bounce

- remind\_return\_path

    (Default value: `owner`)

    Remind message return-path ( unique | owner )

    If set to unique, subcriber is removed if remind message bounce, use with care

- return\_path\_suffix

    (Default value: `-owner`)

- bounce\_path

    (Default value: `/home/sympa/bounce`)

    Directory for storing bounces

    Better if not in a critical partition

- expire\_bounce\_task

    (Default value: `daily`)

    Task name for expiration of old bounces

- purge\_orphan\_bounces\_task

    (Default value: `monthly`)

- eval\_bouncers\_task

    (Default value: `daily`)

- process\_bouncers\_task

    (Default value: `weekly`)

- minimum\_bouncing\_count

    (Default value: `10`)

- minimum\_bouncing\_period

    (Default value: `10`)

- bounce\_delay

    (Default value: `0`)

- default\_bounce\_level1\_rate

    (Default value: `45`)
    (Overridable by each `robot.conf`)

- default\_bounce\_level2\_rate

    (Default value: `75`)
    (Overridable by each `robot.conf`)

- bounce\_email\_prefix

    (Default value: `bounce`)

- bounce\_warn\_rate

    (Default value: `30`)

    Bouncing email rate for warn list owner

- bounce\_halt\_rate

    (Default value: `50`)

    Bouncing email rate for halt the list (not implemented)

    Not yet used in current version, Default is 50

- tracking\_default\_retention\_period

    (Default value: `90`)

- tracking\_delivery\_status\_notification

    (Default value: `off`)

- tracking\_message\_disposition\_notification

    (Default value: `off`)

- default\_remind\_task

    (Optioal)

### Tuning

- cache\_list\_config

    (Default value: `none`)

    Use of binary version of the list config structure on disk (none | binary\_file)

    Set this parameter to "binary\_file" if you manage a big amount of lists (1000+); it should make the web interface startup faster

- sympa\_priority

    (Default value: `1`)
    (Overridable by each `robot.conf`)

    Sympa commands priority

- request\_priority

    (Default value: `0`)
    (Overridable by each `robot.conf`)

- owner\_priority

    (Default value: `9`)
    (Overridable by each `robot.conf`)

- default\_list\_priority

    (Default value: `5`)
    (Overridable by each `robot.conf`)

    Default priority for list messages

- parsed\_family\_files

    (Default value: `message.footer,message.header,message.footer.mime,message.header.mime,info`)
    (Overridable by each `robot.conf`)

    comma-separated list of files that will be parsed by Sympa when instantiating a family (no space allowed in file names)

- incoming\_max\_count

    (Default value: `1`)

    Max number of daemons processing incoming spool that will run on the same server

### Database related

- update\_db\_field\_types

    (Default value: `auto`)

- db\_type

    (Default value: `mysql`)

    Type of the database (mysql|Pg|Oracle|Sybase|SQLite)

    Be careful to the case

- db\_name

    (Default value: `sympa`)

    Name of the database

    With SQLite, the name of the DB corresponds to the DB file

- db\_host

    (Default value: `localhost`)

    Hostname of the database server

    Example:

        db_host localhost

- db\_port

    (Optioal)

    Port of the database server

- db\_user

    (Default value: `user_name`)

    User for the database connection

    Example:

        db_user sympa

- db\_passwd

    (Default value: `user_password`)

    Password for the database connection

    Example:

        db_passwd your_passwd

    What ever you use a password or not, you must protect the SQL server (is it not a public internet service ?)

- db\_timeout

    (Optioal)

- db\_options

    (Optioal)

- db\_env

    (Optioal)

    Environment variables setting for database

    This is useful for defining ORACLE\_HOME 

- db\_additional\_subscriber\_fields

    (Optioal)

    Database private extension to subscriber table

    Example:

        db_additional_subscriber_fields billing_delay,subscription_expiration

    You need to extend the database format with these fields

- db\_additional\_user\_fields

    (Optioal)

    Database private extension to user table

    Example:

        db_additional_user_fields age,address

    You need to extend the database format with these fields

- purge\_user\_table\_task

    (Default value: `monthly`)

- purge\_spools\_task

    (Default value: `daily`)

- purge\_tables\_task

    (Default value: `daily`)

- purge\_logs\_table\_task

    (Default value: `daily`)

- logs\_expiration\_period

    (Default value: `3`)

    Number of months that elapse before a log is expired

- stats\_expiration\_period

    (Default value: `3`)

    Number of months that elapse before statistics are expired

- purge\_one\_time\_ticket\_table\_task

    (Default value: `daily`)

- one\_time\_ticket\_table\_ttl

    (Default value: `10d`)

- purge\_session\_table\_task

    (Default value: `daily`)

- session\_table\_ttl

    (Default value: `2d`)

- anonymous\_session\_table\_ttl

    (Default value: `1h`)

- purge\_challenge\_table\_task

    (Default value: `daily`)

- challenge\_table\_ttl

    (Default value: `5d`)

- default\_ttl

    (Default value: `3600`)

    Default timeout between two scheduled synchronizations of list members with data sources.

- default\_distribution\_ttl

    (Default value: `300`)

    Default timeout between two action-triggered synchronizations of list members with data sources.

- default\_sql\_fetch\_timeout

    (Default value: `300`)

    Default timeout while performing a fetch for an include\_sql\_query sync

### Loop prevention

- loop\_command\_max

    (Default value: `200`)

- loop\_command\_sampling\_delay

    (Default value: `3600`)

- loop\_command\_decrease\_factor

    (Default value: `0.5`)

- loop\_prevention\_regex

    (Default value: `mailer-daemon|sympa|listserv|majordomo|smartlist|mailman`)
    (Overridable by each `robot.conf`)

- msgid\_table\_cleanup\_ttl

    (Default value: `86400`)

- msgid\_table\_cleanup\_frequency

    (Default value: `3600`)

### S/MIME configuration

- capath

    (Optioal)

    Directory containing trusted CA certificates

- cafile

    (Optioal)

    File containing trusted CA certificates

- ssl\_cert\_dir

    (Default value: `/home/sympa/list_data/X509-user-certs`)

    Directory containing user certificates

- key\_passwd

    (Optioal)

    Password used to crypt lists private keys

    Example:

        key_passwd your_password

### DKIM

- dkim\_feature

    (Default value: `off`)
    (Overridable by each `robot.conf`)

- dkim\_add\_signature\_to

    (Default value: `robot,list`)
    (Overridable by each `robot.conf`)

    Insert a DKIM signature to message from the robot, from the list or both

- dkim\_signature\_apply\_on

    (Default value: `md5_authenticated_messages,smime_authenticated_messages,dkim_authenticated_messages,editor_validated_messages`)
    (Overridable by each `robot.conf`)

    Type of message that is added a DKIM signature before distribution to subscribers. Possible values are "none", "any" or a list of the following keywords: "md5\_authenticated\_messages", "smime\_authenticated\_messages", "dkim\_authenticated\_messages", "editor\_validated\_messages".

- dkim\_private\_key\_path

    (Optioal)
    (Overridable by each `robot.conf`)

    Location of the file where DKIM private key is stored

- dkim\_signer\_domain

    (Optioal)
    (Overridable by each `robot.conf`)

    The "d=" tag as defined in rfc 4871, default is virtual host domain name

- dkim\_selector

    (Optioal)
    (Overridable by each `robot.conf`)

    The selector

- dkim\_signer\_identity

    (Optioal)
    (Overridable by each `robot.conf`)

    The "i=" tag as defined in rfc 4871, default is null

- dmarc\_protection\_mode

    (Optioal)
    (Overridable by each `robot.conf`)

    Test mode(s) for DMARC Protection

    Example:

        dmarc_protection_mode dmarc_reject,dkim_signature

    Do not set unless you want to use DMARC protection.  This is a comma separated list of test modes; if multiple are selected then protection is activated if ANY match.  Do not use dmarc\_\* modes unless you have a local DNS cache as they do a DNS lookup for each received message.

- dmarc\_protection\_domain\_regex

    (Optioal)
    (Overridable by each `robot.conf`)

    Regexp for domain name match

    This is used for the "domain\_regex" protection mode.

- dmarc\_protection\_phrase

    (Default value: `name_via_list`)
    (Overridable by each `robot.conf`)

    Pattern used to create new From header phrase

- dmarc\_protection\_other\_email

    (Optioal)
    (Overridable by each `robot.conf`)

    Email to use for replacement From header

### Antivirus plug-in

- antivirus\_path

    (Optioal)
    (Overridable by each `robot.conf`)

    Path to the antivirus scanner engine

    Example:

        antivirus_path /usr/local/bin/clamscan

    supported antivirus: Clam AntiVirus/clamscan & clamdscan, McAfee/uvscan, Fsecure/fsav, Sophos, AVP and Trend Micro/VirusWall

- antivirus\_args

    (Optioal)
    (Overridable by each `robot.conf`)

    Antivirus plugin command argument

    Example:

        antivirus_args --no-summary --database /usr/local/share/clamav

- antivirus\_notify

    (Default value: `sender`)
    (Overridable by each `robot.conf`)

    Notify sender if virus checker detects malicious content

    "sender" to notify originator of the message, "delivery\_status" to send delivery status, or "none"

### Tag based spam filtering

- antispam\_feature

    (Default value: `off`)
    (Overridable by each `robot.conf`)

- antispam\_tag\_header\_name

    (Default value: `X-Spam-Status`)
    (Overridable by each `robot.conf`)

    If a spam filter (like spamassassin or j-chkmail) add a smtp headers to tag spams, name of this header (example X-Spam-Status)

- antispam\_tag\_header\_spam\_regexp

    (Default value: `^\s*Yes`)
    (Overridable by each `robot.conf`)

    Regexp applied on this header to verify message is a spam (example Yes)

- antispam\_tag\_header\_ham\_regexp

    (Default value: `^\s*No`)
    (Overridable by each `robot.conf`)

    Regexp applied on this header to verify message is NOT a spam (example No)

- spam\_status

    (Default value: `x-spam-status`)
    (Overridable by each `robot.conf`)

    Value of this parameter is name of `spam_status` scenario.

    Messages are supposed to be filtered by an antispam that add one more headers to messages. This parameter is used to select a special scenario in order to decide the message spam status: ham, spam or unsure. This parameter replace antispam\_tag\_header\_name, antispam\_tag\_header\_spam\_regexp and antispam\_tag\_header\_ham\_regexp.

### Web interface parameters

- arc\_path

    (Default value: `/home/sympa/arc`)
    (Overridable by each `robot.conf`)

    Directory for storing HTML archives

    Better if not in a critical partition

- archive\_default\_index

    (Default value: `thrd`)

    Default index organization when entering the web archive: either threaded or in chronological order

- cookie\_expire

    (Default value: `0`)

    HTTP cookies lifetime

- cookie\_domain

    (Default value: `localhost`)
    (Overridable by each `robot.conf`)

    HTTP cookies validity domain

- cookie\_refresh

    (Default value: `60`)

    Average interval to refresh HTTP session ID.

- custom\_archiver

    (Optioal)

    Activates a custom archiver to use instead of MHonArc. The value of this parameter is the absolute path on the file system to the script of the custom archiver.

- default\_home

    (Default value: `home`)
    (Overridable by each `robot.conf`)

    Type of main Web page ( lists | home )

- edit\_list

    (Default value: `owner`)

- ldap\_force\_canonical\_email

    (Default value: `1`)
    (Overridable by each `robot.conf`)

    When using LDAP authentication, if the identifier provided by the user was a valid email, if this parameter is set to false, then the provided email will be used to authenticate the user. Otherwise, use of the first email returned by the LDAP server will be used.

- log\_facility

    (Default value: `LOCAL1`)

    Syslog facility for wwsympa, archived and bounced

    Default is to use previously defined sympa log facility.

- mhonarc

    (Default value: `/usr/bin/mhonarc`)
    (Overridable by each `robot.conf`)

    Path to MHonArc mail2html plugin

    This is required for HTML mail archiving

- htmlarea\_url

    (Optioal)

- one\_time\_ticket\_lifetime

    (Default value: `2d`)

    duration before the one time tickets are expired

- one\_time\_ticket\_lockout

    (Default value: `one_time`)
    (Overridable by each `robot.conf`)

    Is access to the one time ticket restricted, if any users previously accessed? (one\_time | remote\_addr | open)

- password\_case

    (Default value: `insensitive`)

    Password case (insensitive | sensitive)

    Should not be changed ! May invalid all user password

- review\_page\_size

    (Default value: `25`)
    (Overridable by each `robot.conf`)

    Default number of lines of the array displaying users in the review page

- title

    (Default value: `Mailing lists service`)
    (Overridable by each `robot.conf`)

    Title of main Web page

- use\_html\_editor

    (Default value: `0`)
    (Overridable by each `robot.conf`)

    If set to "on", users will be able to post messages in HTML using a javascript WYSIWYG editor.

- html\_editor\_url

    (Optioal)
    (Overridable by each `robot.conf`)

    URL path to the javascript file making the WYSIWYG HTML editor available.  Relative path under &lt;static\_content\_url> or absolute path

    Example:

        html_editor_url js/tinymce/tinymce.min.js

    Example is for TinyMCE 4 installed under &lt;static\_content\_path>/js/tinymce/.

- html\_editor\_init

    (Optioal)
    (Overridable by each `robot.conf`)

    Javascript excerpt that enables and configures the WYSIWYG HTML editor.

    Example:

        html_editor_init tinymce.init({selector:"#body",language:lang.split(/[^a-zA-Z]+/).join("_")});

- use\_fast\_cgi

    (Default value: `1`)

    Is FastCGI module for HTTP server installed (0 | 1)

    This module provide much faster web interface

- viewlogs\_page\_size

    (Default value: `25`)
    (Overridable by each `robot.conf`)

    Default number of lines of the array displaying the log entries in the logs page

- http\_host

    (Default value: `host.domain.tld`)
    (Overridable by each `robot.conf`)

    Web domain of a virtual host

    Example:

        http_host host.domain.tld

- password\_validation

    (Optioal)

    The password validation techniques to be used against user passwords that are added to mailing lists. Options come from Data::Password (http://search.cpan.org/~razinf/Data-Password-1.07/Password.pm#VARIABLES)

    Example:

        password_validation MINLEN=8,GROUPS=3,DICTIONARY=4,DICTIONARIES=/pentest/dictionaries

# FILES

- `/etc/sympa/sympa.conf`

    Sympa main configuration file.

- `/home/sympa/etc/<robot name>/robot.conf`

    Configuration specific to each virtual host.

# SEE ALSO

_Sympa, Mailing List Management Software - Reference Manual_.
[http://www.sympa.org/manual/](http://www.sympa.org/manual/).
