# NAME

Sympa - Future base class of Sympa functional objects

# DESCRIPTION

This module aims to be the base class for functional objects of Sympa:
Site, Robot, Family and List.

## Functions

### Finding config files and templates

- search\_fullpath ( $that, $name, \[ opt => val, ...\] )

        # To get file name for global site
        $file = Sympa::search_fullpath('*', $name);
        # To get file name for a robot
        $file = Sympa::search_fullpath($robot_id, $name);
        # To get file name for a family
        $file = Sympa::search_fullpath($family, $name);
        # To get file name for a list
        $file = Sympa::search_fullpath($list, $name);

    Look for a file in the list > robot > site > default locations.

    Possible values for options:
        order     => 'all'
        subdir    => directory ending each path
        lang      => language
        lang\_only => if paths without lang subdirectory would be omitted

    Returns full path of target file `_root_/_subdir_/_lang_/_name_`
    or `_root_/_subdir_/_name_`.
    _root_ is the location determined by target object $that.
    _subdir_ and _lang_ are optional.
    If `lang_only` option is set, paths without _lang_ subdirectory is omitted.

- get\_search\_path ( $that, \[ opt => val, ... \] )

        # To make include path for global site
        @path = @{Sympa::get_search_path('*')};
        # To make include path for a robot
        @path = @{Sympa::get_search_path($robot_id)};
        # To make include path for a family
        @path = @{Sympa::get_search_path($family)};
        # To make include path for a list
        @path = @{Sympa::get_search_path($list)};

    make an array of include path for tt2 parsing

    IN :
          -$that(+) : ref(Sympa::List) | ref(Sympa::Family) | Robot | "\*"
          -%options : options

    Possible values for options:
        subdir    => directory ending each path
        lang      => language
        lang\_only => if paths without lang subdirectory would be omitted

    OUT : ref(ARRAY) of tt2 include path

### Sending Notifications

- send\_dsn ( $that, $message,
\[ { key => val, ... }, \[ $status, \[ $diag \] \] \] )

        # To send site-wide DSN
        Sympa::send_dsn('*', $message, {'recipient' => $rcpt},
            '5.1.2', 'Unknown robot');
        # To send DSN related to a robot
        Sympa::send_dsn($robot, $message, {'listname' => $name},
            '5.1.1', 'Unknown list');
        # To send DSN specific to a list
        Sympa::send_dsn($list, $message, {}, '2.1.5', 'Success');

    Sends a delivery status notification (DSN) to SENDER
    by parsing delivery\_status\_notification.tt2 template.

- send\_file ( $that, $tpl, $who, \[ $context, \[ options... \] \] )

        # To send site-global (not relative to a list or a robot)
        # message
        Sympa::send_file('*', $template, $who, ...);
        # To send global (not relative to a list, but relative to a
        # robot) message
        Sympa::send_file($robot, $template, $who, ...);
        # To send message relative to a list
        Sympa::send_file($list, $template, $who, ...);

    Send a message to user(s).
    Find the tt2 file according to $tpl, set up
    $data for the next parsing (with $context and
    configuration)
    Message is signed if the list has a key and a
    certificate

    Note: List::send\_global\_file() was deprecated.

- send\_notify\_to\_listmaster ( $that, $operation, $data )

        # To send notify to super listmaster(s)
        Sympa::send_notify_to_listmaster('*', 'css_updated', ...);
        # To send notify to normal (per-robot) listmaster(s)
        Sympa::send_notify_to_listmaster($robot, 'web_tt2_error', ...);
        # To send notify to normal listmaster(s) of robot the list belongs to.
        Sympa::send_notify_to_listmaster($list, 'request_list_creation', ...);

    Sends a notice to (super or normal) listmaster by parsing
    listmaster\_notification.tt2 template.

    Parameters:

    - $self

        [Sympa::List](./Sympa::List.3.md), Robot or Site.

    - $operation

        Notification type.

    - $param

        Hashref or arrayref.
        Values for template parsing.

    Returns:

    `1` or `undef`.

- send\_notify\_to\_user ( $that, $operation, $user, $param )

    Send a notice to a user (sender, subscriber or another user)
    by parsing user\_notification.tt2 template.

    Parameters:

    - $that

        [Sympa::List](./Sympa::List.3.md), Robot or Site.

    - $operation

        Notification type.

    - $user

        E-mail of notified user.

    - $param

        Hashref or arrayref.  Values for template parsing.

    Returns:

    `1` or `undef`.

### Internationalization

- best\_language ( LANG, ... )

        # To get site-wide best language.
        $lang = Sympa::best_language('*', 'de', 'en-US;q=0.9');
        # To get robot-wide best language.
        $lang = Sympa::best_language($robot, 'de', 'en-US;q=0.9');
        # To get list-specific best language.
        $lang = Sympa::best_language($list, 'de', 'en-US;q=0.9');

    Chooses best language under the context of List, Robot or Site.
    Arguments are language codes (see [Language](https://metacpan.org/pod/Language)) or ones with quality value.
    If no arguments are given, the value of `HTTP_ACCEPT_LANGUAGE` environment
    variable will be used.

    Returns language tag or, if negotiation failed, lang of object.

- get\_supported\_languages ( $that )

    _Function_.
    Gets supported languages, canonicalized.
    In array context, returns array of supported languages.
    In scalar context, returns arrayref to them.

### Addresses and users

These are accessors derived from configuration parameters.

- get\_address ( $that, \[ $type \] )

        # Get address bound for super listmaster(s).
        Sympa::get_address('*', 'listmaster');     # <listmaster@DEFAULT_HOST>
        # Get address for command robot and robot listmaster(s).
        Sympa::get_address($robot, 'sympa');       # <sympa@HOST>
        Sympa::get_address($robot, 'listmaster');  # <listmaster@HOST>
        # Get address for command robot and robot listmaster(s).
        Sympa::get_address($family, 'sympa');      # <sympa@HOST>
        Sympa::get_address($family, 'listmaster'); # listmaster@HOST>
        # Get address bound for the list and its owner(s) etc.
        Sympa::get_address($list);                 # <NAME@HOST>
        Sympa::get_address($list, 'owner');        # <NAME-request@HOST>
        Sympa::get_address($list, 'editor');       # <NAME-editor@HOST>
        Sympa::get_address($list, 'return_path');  # <NAME-owner@HOST>

    Site or robot:
    Returns the site or robot email address of type $type: email command address
    (default, &lt;sympa> address), "owner" (&lt;sympa-request> address) or "listmaster".

    List:
    Returns the list email address of type $type: posting address (default),
    "owner" (<LIST-request> address), "editor", non-VERP "return\_path"
    (<LIST-owner> address), "subscribe" or "unsubscribe".

    Note:
    %Conf::Conf or Conf::get\_robot\_conf() may return &lt;sympa> and
    &lt;sympa-request> addresses by "sympa" and "request" arguments, respectively.
    They are obsoleted.  Use this function instead.

- get\_listmasters\_email ( $that )

        # To get addresses of super-listmasters.
        @addrs = Sympa::get_listmasters_email('*');
        # To get addresses of normal listmasters of a robot.
        @addrs = Sympa::get_listmasters_email($robot);
        # To get addresses of normal listmasters of the robot of a family.
        @addrs = Sympa::get_listmasters_email($family);
        # To get addresses of normal listmasters of the robot of a list.
        @addrs = Sympa::get_listmasters_email($list);

    Gets valid email addresses of listmasters. In array context, returns array of
    addresses. In scalar context, returns arrayref to them.

- get\_url ( $that, $action, \[ nomenu => 1 \], \[ paths => \\@paths \],
\[ authority => $mode \],
\[ options... \] )

    Returns URL for web interface.

    Parameters:

    - $action

        Name of action.
        This is inserted into URL intact.

    - authority => $mode

        `'default'` respects `wwsympa_url` parameter.
        `'local'` is similar but may replace host name and script path.
        `'omit'` omits scheme and authority, i.e. returns relative URI.

        Note that `'local'` mode works correctly only under CGI environment.
        See also a note below.

    - nomenu => 1

        Adds `nomenu` modifier.

    - paths => \\@paths

        Additional path components.
        Note that they are percent-encoded as necessity.

    - options...

        See ["weburl" in Sympa::Tools::Text](./Sympa::Tools::Text.3.md#weburl).

    Returns:

    A string.

    Note:
    If $mode is `'local'`, result is that Sympa server recognizes locally.
    In other cases, result is the URI that is used by end users to access to web
    interface.
    When, for example, the server is placed behind a reverse-proxy,
    `Location:` field in HTTP response to cause redirection would be better
    to contain `'local'` URI.

- is\_listmaster ( $that, $who )

    Is the user listmaster?

- unique\_message\_id ( $that )

    TBD

# SEE ALSO

[Sympa::Site](./Sympa::Site.3.md) (not yet available),
[Sympa::Robot](./Sympa::Robot.3.md) (not yet available),
[Sympa::Family](./Sympa::Family.3.md),
[Sympa::List](./Sympa::List.3.md).
