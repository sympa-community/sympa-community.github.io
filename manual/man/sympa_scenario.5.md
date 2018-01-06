---
title: 'sympa_scenario(5)'
---

# NAME

sympa\_scenario - Authorization scenario

# SYNOPSIS

An example `del.auth` file:

    title    deletion performed only by list owners, need authentication
    title.es eliminacin reservada slo para el propietario, necesita autentificacin
    
    is_owner([listname],[sender])  smtp       -> request_auth
    is_listmaster([sender])        smtp       -> request_auth
    true()                         md5,smime  -> do_it

# DESCRIPTION

## File format

Basically, a scenario file is composed of titles on the first lines and a set
of rules on the following lines.

Rules consist of one or more line in the form:

    condition authentication_methods -> action

Some terms of conditions may take one or more arguments.
The arguments are variables or literals (see ["Terms of conditions"](#terms-of-conditions),
["Variables"](#variables)).

Authentication methods is a comma-separated list of one or more methods
(see ["Authentication methods"](#authentication-methods)).

Some actions may have optional modifiers (see ["Actions"](#actions)).

### Terms of conditions

- `true` `(` `)`

    Always returns true.

- `equal` `(` _var1_`,` _var2_ `)`

    Tests if two arguments are equial.

- `is_subscriber` `(` _listname_`,` _var_ `)`
- `is_owner` `(` _listname_`,` _var_ `)`
- `is_editor` `(` _listname_`,` _var_ `)`

    Tests if _var_ is the subscriber, owner or editor of the list _listname_.
    _listname_ is the variable `[listname]` or list address, "_name_" or
    "_name_`@`_domain_".

- `is_listmaster` `(` _var_ `)`

    Tests if _var_ is the listmaster. 

- `less_than` `(` _var1_`,` _var2_ `)`

    Tests if _var1_ is less than _var2_.

- `match` `(` _var_`,` `/`_perl\_regexp_`/` `)`

    Tests if _var_ matches with _perl\_regexp_.

    _perl\_regexp_ is a perl regular expression.
    Don't forget to escape special characters (`^`, `$`, `{`, `(`, ...):
    Check [perlre(1)](./perlre.1.md) for regular expression syntax.
    It can contain the string `[host]` (interpreted at run time as the list or
    robot domain). 

- `newer` `(` _date_`,` _date_ `)`

    Returns true if first date is posterior to the second 

- `older` `(` _date_`,` _date_ `)`

    Returns true if first date is anterior to the second date

    _date_ is Unix time or the string
    "_n_`y`_n_`m`_n_`d`_n_`h`_n_`min`_n_`sec`", where each _n_ is a
    number.

- `search` `(` _named\_filter\_file_`,` _var_`)`

    Tests if _var_ is found by named filter.

    _named\_filter\_file_ is a file name ending with `.ldap`, `.sql` or `.txt`.

- `verify_netmask` `(` _network\_block_ `)`

    Tests if `REMOTE_ADDR` CGI environment variable matches with
    _network\_block_.

    This allows the user to configure their local network to only be accessible
    to those that are members of it.

- `CustomCondition::`_package\_name_ `(` _arguments_, ... `)`

    Evaluates custom condition.
    _package\_name_ is the name of a Perl package in
    `$SYSCONFDIR/custom_conditions/` (lowercase).

### Variables

- `[custom_vars->`_custom\_var\_name_`]`

    Allows you to introduce custom parameters in your scenario.
    _custom\_var\_name_ is the name of the custom parameter you want to use.

- `[date]`

    Date of reception of the message.

- `[env->`_env\_var_`]` 

    _env\_var_ is the name of CGI environment variable (note that it is
    case-sensitive).

- `[is_bcc]`

    Set to 1 if the list is neither in To: nor Cc: field.

- `[listname]`

    Name of current list.

- `[msg_encrypted]`

    Set to "`smime`" if the message was S/MIME encrypted.

- `[msg_header->`_field\_name_`]`
- `[msg_header->`_field\_name_`][`_index_`]`

    Value of message header field, available only when evaluating the
    authorization scenario for sending messages.
    It can be used, for example, to require editor validation for multipart
    messages.
    Optional _index_ may be integer (may be less than `0`) to choose particular
    entry from multile fields.

- `[msg_part->type]`
- `[msg_part->body]`

    The MIME content types and bodies; the body is available for MIME parts
    in text/xxx format only.

- `[previous_email]`

    Old email when changing subscription email in preference page.

- `[sender]`

    The email address of the current user (used on web or mail interface).
    Default value is "nobody".

- `[topic]`

    Topic of the message.
    This variable has a value if any of the previous
    `[topic-*]` variable has a value.

- `[topic-auto]`

    Topic of the message if it has been automatically tagged.

- `[topic-editor]`

    Topic of the message if it has been tagged by editor.

- `[topic-needed]`

    The message has not got any topic and message topic are required for the list.

- `[topic-sender]`

    Topic of the message if it has been tagged by sender.

- `[user_attributes->`_user\_attributes\_key\_word_`]`

    _user\_attributes\_key\_word_ is one of the names of user attributes provided
    by the SSO system via environment variables.
    Available only if user authenticated with a `generic_sso`.

### Authentication methods

The e-mail of authenticated user is given by `[sender]` variable.
If it is not given, '`nobody`' will be set.

- `smtp`

    Default method.
    No actual authentication, and if any, sender of the message is used.

- `dkim`

    Authenticated by DKIM signature.

- `md5`

    Authenticated by web authentication (password),
    or by authentication key in e-mail message.

- `smime`

    Authenticated by S/MIME signature,
    or TLS client certificate.

### Actions

An action consists of an action name and optional modifiers.

Action names:

- `do_it`

    Allows operation.

- `editor`

    The message will be forwarded to list editor.

- `editorkey`

    The message will be held for moderation by list editor.

- `listmaster`

    Same as `do_it` but makes newly created list be pending.

- `owner`

    The operation is held and waits for approval by list owner.

- `reject`

    Denys operation.

- `request_auth`

    The operation is held and waits for confirmation by the user.

Modifiers:

- `([email])`

    Only for `request_auth` action.
    Sends authentication request to the target user of operation (given as the
    value of "`[email]`" variable) instead of original sender.

    Note that `[email]` is a literal and no other variable names can't be used.

- `,` `notify`

    Only for `do_it` and `listmaster` actions.
    Sends a notification to list owner.

- `,` `quiet`

    Sends no notification to the message sender.

- `(reason='`_reason\_key_`')`

    Only for `reject` action.
    Matches a key in `mail_tt2/authorization_reject.tt2` template corresponding
    to an information message about the reason of the reject of the user.
    _reason\_key_ have to be a static string enclosed by `'...'`.

- `(tt2='`_tpl\_name_`')`

    Only for `reject` action.
    Corresponding template (_tpl\_name_`.tt2`) is sent to the sender.
    _tpl\_name_ have to be a static string enclosed by `'...'`.

## Formal syntax

\# Below is the formal syntax definition by modified BNF.

rule : condition spaces auth\_list "->" action

\# Condition

condition : "!" condition  
    | "true" "(" ")"  
    | "equal" "(" var "," var ")"  
    | "is\_editor" "(" listname "," var ")"  
    | "is\_listmaster" "(" var ")"  
    | "is\_owner" "(" listname "," var ")"  
    | "is\_subscriber" "(" listname "," var ")"  
    | "less\_than" "(" var "," var ")"  
    | "match" "(" var "," "/" perl\_regexp "/" ")"  
    | "newer" "(" date "," date ")"  
    | "older" "(" date "," date ")"  
    | "search" "(" named\_filter\_file ")"  
    | "verify\_netmask" "(" network\_block ")"  
    | "CustomCondition::" package\_name "(" var\* ")"

var : "\[email\]"  
    | "\[conf->" conf\_key\_word "\]"  
    | "\[current\_date\]"  
    | "\[custom\_vars->" custom\_var\_name "\]"  
    | "\[env->" env\_var "\]"  
    | "\[is\_bcc\]"  
    | "\[list->" list\_key\_word "\]"  
    | "\[msg\_body\]"  
    | "\[msg\_encrypted\]"  
    | "\[msg\_header->" field\_name "\]" "\[" index "\]"  
    | "\[msg\_header->" field\_name "\]"  
    | "\[msg\_part->type\]"  
    | "\[msg\_part->body\]"  
    | "\[previous\_email\]"  
    | "\[sender\]"  
    | "\[subscriber->" subscriber\_key\_word "\]"  
    | "\[topic\]"  
    | "\[topic-auto\]"  
    | "\[topic-editor\]"  
    | "\[topic-needed\]"  
    | "\[topic-sender\]"  
    | "\[user->" user\_key\_word "\]"  
    | "\[user\_attributes->" user\_attributes\_keyword "\]"  
    | string

listname : "\[listname\]"  
    | listname\_string  
    | listname\_string "@" domain\_string

date : "\[date\]"  
    | date\_expr
    | integer

user\_key\_word : "email"
    | "gecos"  
    | "lang"  
    | "password"  
    | "cookie\_delay\_user"  
    | additional\_user\_fields

subscriber\_key\_word : "email"  
    | "date"  
    | "bounce"  
    | "gecos"  
    | "reception"  
    | "update\_date"  
    | "visibility"  
    | additional\_subscriber\_fields

list\_key\_word : "name"  
    | "address"  
    | "host"  
    | "lang"  
    | "max\_size"  
    | "priority"  
    | "reply\_to"  
    | "status"  
    | "subject"  
    | "total"  
    | "account"

conf\_key\_word : "domain"  
    | "default\_list\_priority"  
    | "email"  
    | "lang"  
    | "listmaster"  
    | "max\_size"  
    | "request\_priority"  
    | "sympa\_priority"

\# Authentication methods

auth\_list : auth "," auth\_list  
    | auth  
    | ""

auth : "smtp"  
    | "dkim"  
    | "md5"  
    | "smime"

\# Actions

action : "do\_it" ( "," "quiet" | "," "notify" )\*  
    | "editor" \[ "," "quiet" \]  
    | "editorkey" \[ "," "quiet" \]  
    | "listmaster" \[ "," "notify" \]  
    | "owner" \[ "," "quiet" \]  
    | "reject" (  
          "(" "reason=" reason\_key ")"  
        | "(" "tt2=" tpl\_name ")"  
        | "," "quiet"  
      )\*  
    | "reject(tt2=" tpl\_name ")" \[ "," "quiet" \]  
    | "request\_auth" \[ "(\[email\])" \]

# FILES

- $EXPLDIR`/`_list path_`/scenari`
- $SYSCONFDIR`/`_virtual host_`/scenari`
- $SYSCONFDIR`/scenari`
- $DEFAULTDIR`/scenari`

    Path of scenario files: List, robot and site levels, and distribution defaults.

# SEE ALSO

[Sympa::Scenario](./Sympa-Scenario.3.md).

# HISTORY

Original contents of this document were partially taken from
a chapter "Authorization scenarios" in
_Sympa, Mailing List Management Software - Reference manual_.
