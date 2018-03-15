---
title: 'sympa_database(5)'
---

# NAME

sympa\_database - Structure of Sympa core database

# DECRIPTION

Core database of Sympa is based on SQL.
In following list of tables and indexes, data types are based on
MySQL/MariaDB.  Corresponding types are used by other platforms
(PostgreSQL, SQLite, ...).

## Tables

### subscriber\_table

This table store subscription, subscription option etc.

Fields:

- user\_subscriber varchar(100)

    (Primary key)

    email of subscriber

- list\_subscriber varchar(50)

    (Primary key)

    list name of a subscription

- robot\_subscriber varchar(80)

    (Primary key)

    robot (domain) of the list

- reception\_subscriber varchar(20)

    reception format option of subscriber (digest, summary, etc.)

- suspend\_subscriber int(1)

    boolean set to 1 if subscription is suspended

- suspend\_start\_date\_subscriber int(11)

    the Unix time when message reception is suspended

- suspend\_end\_date\_subscriber int(11)

    the Unix time when message reception should be restored

- bounce\_subscriber varchar(35)

    FIXME

- bounce\_score\_subscriber smallint(6)

    FIXME

- bounce\_address\_subscriber varchar(100)

    FIXME

- date\_epoch\_subscriber int(11) not null

    date of subscription

- update\_epoch\_subscriber int(11)

    the last time when subscription is confirmed by subscriber

- comment\_subscriber varchar(150)

    free form name

- number\_messages\_subscriber int(5) not null

    the number of message the subscriber sent

- visibility\_subscriber varchar(20)

    FIXME

- topics\_subscriber varchar(200)

    topic subscription specification

- subscribed\_subscriber int(1)

    boolean set to 1 if subscriber comes from ADD or SUB

- included\_subscriber int(1)

    boolean, set to 1 is subscriber comes from an external datasource. Note that included\_subscriber and subscribed\_subscriber can both value 1

- include\_sources\_subscriber varchar(50)

    comma separated list of datasource that contain this subscriber

- custom\_attribute\_subscriber text

    FIXME

Indexes:

- subscriber\_user\_index

    user\_subscriber

### user\_table

The user\_table is mainly used to manage login from web interface. A subscriber may not appear in the user\_table if he never log through the web interface.

Fields:

- email\_user varchar(100)

    (Primary key)

    email of user

- password\_user varchar(64)

    password are stored as finger print

- gecos\_user varchar(150)

    display name of user

- last\_login\_date\_user int(11)

    Unix time of last login, printed in login result for security purpose

- last\_login\_host\_user varchar(60)

    host of last login, printed in login result for security purpose

- wrong\_login\_count\_user int(11)

    login attempt count, used to prevent brute force attack

- last\_active\_date\_user int(11)

    the last Unix time when this user was confirmed their activity by purge\_user\_table task

- cookie\_delay\_user int(11)

    FIXME

- lang\_user varchar(10)

    user language preference

- attributes\_user text

    FIXME

- data\_user text

    FIXME

### inclusion\_table

Inclusion table is used in order to manage lists included from / including subscribers of other lists.

Fields:

- target\_inclusion varchar(131)

    (Primary key)

    list ID of including list

- role\_inclusion enum('member','owner','editor')

    (Primary key)

    role of included user

- source\_inclusion varchar(131)

    (Primary key)

    list ID of included list

- update\_epoch\_inclusion int(11)

    the date this entry was created or updated

### exclusion\_table

Exclusion table is used in order to manage unsubscription for subscriber included from an external data source.

Fields:

- list\_exclusion varchar(57)

    (Primary key)

    FIXME

- robot\_exclusion varchar(80)

    (Primary key)

    FIXME

- user\_exclusion varchar(100)

    (Primary key)

    FIXME

- family\_exclusion varchar(50)

    (Primary key)

    FIXME

- date\_exclusion int(11)

    FIXME

### session\_table

Management of HTTP session.

Fields:

- id\_session varchar(30)

    (Primary key)

    the identifier of the database record

- prev\_id\_session varchar(30)

    previous identifier of the database record

- start\_date\_session int(11) not null

    the date when the session was created

- date\_session int(11) not null

    Unix time of the last use of this session. It is used in order to expire old sessions

- refresh\_date\_session int(11)

    Unix time of the last refresh of this session.  It is used in order to refresh available sessions

- remote\_addr\_session varchar(60)

    the IP address of the computer from which the session was created

- robot\_session varchar(80)

    the virtual host in which the session was created

- email\_session varchar(100)

    the email associated to this session

- hit\_session int(11)

    the number of hit performed during this session. Used to detect crawlers

- data\_session text

    parameters attached to this session that don't have a dedicated column in the database

### one\_time\_ticket\_table

One time ticket are random value used for authentication challenge. A ticket is associated with a context which look like a session.

Fields:

- ticket\_one\_time\_ticket varchar(30)

    (Primary key)

    FIXME

- email\_one\_time\_ticket varchar(100)

    FIXME

- robot\_one\_time\_ticket varchar(80)

    FIXME

- date\_one\_time\_ticket int(11)

    FIXME

- data\_one\_time\_ticket varchar(200)

    FIXME

- remote\_addr\_one\_time\_ticket varchar(60)

    FIXME

- status\_one\_time\_ticket varchar(60)

    FIXME

### notification\_table

Used for message tracking feature. If the list is configured for tracking, outgoing messages include a delivery status notification request and optionally a message disposition notification request. When DSN and MDN are received by Sympa, they are stored in this table in relation with the related list and message ID.

Fields:

- pk\_notification bigint(20) auto\_increment

    (Primary key)

    autoincrement key

- message\_id\_notification varchar(100)

    initial message-id. This field is used to search DSN and MDN related to a particular message

- recipient\_notification varchar(100)

    email address of recipient for which a DSN or MDN was received

- reception\_option\_notification varchar(20)

    the subscription option of the subscriber when the related message was sent to the list. Useful because some recipient may have option such as //digest// or //nomail//

- status\_notification varchar(100)

    value of notification

- arrival\_date\_notification varchar(80)

    reception date of latest DSN or MDN

- arrival\_epoch\_notification int(11)

    reception date of latest DSN or MDN

- type\_notification enum('DSN', 'MDN')

    type of the notification (DSN or MDN)

- list\_notification varchar(50)

    the listname the message was issued for

- robot\_notification varchar(80)

    the robot the message is related to

- date\_notification int(11) not null

    FIXME

### logs\_table

Each important event is stored in this table. List owners and listmaster can search entries in this table using web interface.

Fields:

- user\_email\_logs varchar(100)

    e-mail address of the message sender or email of identified web interface user (or soap user)

- date\_logs int(11) not null

    date when the action was executed

- usec\_logs int(6)

    subsecond in microsecond when the action was executed

- robot\_logs varchar(80)

    name of the robot in which context the action was executed

- list\_logs varchar(50)

    name of the mailing-list in which context the action was executed

- action\_logs varchar(50) not null

    name of the Sympa subroutine which initiated the log

- parameters\_logs varchar(100)

    comma-separated list of parameters. The amount and type of parameters can differ from an action to another

- target\_email\_logs varchar(100)

    e-mail address (if any) targeted by the message

- msg\_id\_logs varchar(255)

    identifier of the message which triggered the action

- status\_logs varchar(10) not null

    exit status of the action. If it was an error, it is likely that the error\_type\_logs field will contain a description of this error

- error\_type\_logs varchar(150)

    name of the error string - if any - issued by the subroutine

- client\_logs varchar(100)

    IP address of the client machine from which the message was sent

- daemon\_logs varchar(10) not null

    name of the Sympa daemon which ran the action

### stat\_table

Statistics item are stored in this table, Sum average and so on are stored in stat\_counter\_table.

Fields:

- date\_stat int(11) not null

    FIXME

- email\_stat varchar(100)

    FIXME

- operation\_stat varchar(50) not null

    FIXME

- list\_stat varchar(50)

    FIXME

- daemon\_stat varchar(20)

    FIXME

- user\_ip\_stat varchar(100)

    FIXME

- robot\_stat varchar(80) not null

    FIXME

- parameter\_stat varchar(50)

    FIXME

- read\_stat tinyint(1) not null

    FIXME

Indexes:

- stats\_user\_index

    email\_stat

### stat\_counter\_table

Used in conjunction with stat\_table for users statistics.

Fields:

- end\_date\_counter int(11)

    FIXME

- beginning\_date\_counter int(11) not null

    FIXME

- data\_counter varchar(50) not null

    FIXME

- robot\_counter varchar(80) not null

    FIXME

- list\_counter varchar(50)

    FIXME

- count\_counter int

    FIXME

### admin\_table

This table is an internal cash where list admin roles are stored. It is just a cash and it does not need to be saved. You may remove its content if needed. It will just make next Sympa startup slower.

Fields:

- user\_admin varchar(100)

    (Primary key)

    list admin email

- list\_admin varchar(50)

    (Primary key)

    list name

- robot\_admin varchar(80)

    (Primary key)

    list domain

- role\_admin enum('listmaster','owner','editor')

    (Primary key)

    a role of this user for this list (editor, owner or listmaster which a kind of list owner too)

- profile\_admin enum('privileged','normal')

    privilege level for this owner, value //normal// or //privileged//. The related privilege are listed in edit\_list.conf. 

- date\_epoch\_admin int(11) not null

    date this user become a list admin

- update\_epoch\_admin int(11)

    last update time

- reception\_admin varchar(20)

    email reception option for list management messages

- visibility\_admin varchar(20)

    admin user email can be hidden in the list web page description

- comment\_admin varchar(150)

    FIXME

- subscribed\_admin int(1)

    set to 1 if user is list admin by definition in list config file

- included\_admin int(1)

    set to 1 if user is admin by an external data source. Note that included\_admin and subscribed\_admin can both value 1

- include\_sources\_admin varchar(50)

    name of external datasource

- info\_admin varchar(150)

    private information usually dedicated to listmasters who needs some additional information about list owners

Indexes:

- admin\_user\_index

    user\_admin

### netidmap\_table

FIXME

Fields:

- netid\_netidmap varchar(100)

    (Primary key)

    FIXME

- serviceid\_netidmap varchar(100)

    (Primary key)

    FIXME

- robot\_netidmap varchar(80)

    (Primary key)

    FIXME

- email\_netidmap varchar(100)

    FIXME

### conf\_table

FIXME

Fields:

- robot\_conf varchar(80)

    (Primary key)

    FIXME

- label\_conf varchar(80)

    (Primary key)

    FIXME

- value\_conf varchar(300)

    the value of parameter //label\_conf// of robot //robot\_conf//.

### list\_table

The list\_table holds cached list config and some items to help searching lists.

Fields:

- name\_list varchar(50)

    (Primary key)

    name of the list

- robot\_list varchar(80)

    (Primary key)

    name of the robot (domain) the list belongs to

- family\_list varchar(50)

    name of the family the list belongs to

- status\_list enum('open','closed','pending','error\_config','family\_closed')

    status of the list

- creation\_email\_list varchar(100)

    email of user who created the list

- creation\_epoch\_list int(11)

    UNIX time when the list was created

- update\_email\_list varchar(100)

    email of user who updated the list

- update\_epoch\_list int(11)

    UNIX time when the list was updated

- searchkey\_list varchar(255)

    case-folded list subject to help searching

- web\_archive\_list tinyint(1)

    if the list has archives

- topics\_list varchar(255)

    topics of the list, separated and enclosed by commas

- total\_list int(7)

    estimated number of subscribers

# SEE ALSO

_Sympa and its database_.
[https://www.sympa.org/manual/database](https://www.sympa.org/manual/database).
