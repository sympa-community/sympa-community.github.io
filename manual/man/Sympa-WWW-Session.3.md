---
title: 'Sympa::WWW::Session(3)'
---

# NAME

Sympa::WWW::Session - Web session

# SYNOPSIS

    use Sympa::WWW::Session;
    
    my $session = Sympa::WWW::Session->new($robot,
        {cookie => Sympa::WWW::Session::get_session_cookie($ENV{'HTTP_COOKIE'})}
    );
    $session->renew();
    $session->store();

## Confirmation

    $session->confirm_action($action, 'init');
    
    sub do_myaction {
    
        # Validate arguments...
    
        $param->{arg} = $arg;
        my $next_action = $session->confirm_action($action, $response,
            $arg, $previous_action);
        return $next_action unless $next_action eq '1';
    
        # Process action...
    
    }

# DESCRIPTION

[Sympa::WWW::Session](./Sympa-WWW-Session.3.md) provides web session for Sympa web interface.
HTTP cookie is required to determine users.
Session store is used to keep users' personal data.

## Methods

- new ( $robot, { \[ cookie => $cookie \], ... } )

    _Constructor_.
    Creates new instance and loads user data from session store.

    Parameters:

    - $robot

        Context of the session.

    - { cookie => $cookie }

        HTTP cookie.

    Returns:

    A new instance.

- as\_hashref ( )

    _Instance method_.
    Casts the instance to hashref.

    Parameters:

    None.

    Returns:

    A hashref including attributes of instance (see ["Attributes"](#attributes)).

- confirm\_action ( $action, $response, \[ arg => $arg, \]
\[ previous\_action => $previous\_action \] )

    _Instance method_.
    Check if action has been confirmed.

    Confirmation follows two steps:

    1. The method is called with no (undefined) response.
    The action, hash of argument and previous\_action are stored into
    session store.
    And then this method returns `'confirm_action'`.
    2. The method is called with `'confirm'` or other true value as response.
    _If_ action and hash of argument match with those in session store, and:

        - If `'confirm'` is given, returns `1`.
        - If other true value is given, returns previous action stored in
        session store (previous\_action given in argument is ignored).

        In both cases session store is cleared.

    Anytime when the action submitted by user is determined,
    This method may be called with response as `'init'`.
    In this case, if action doesn't match with that in session store,
    session store will be cleared.

    Parameters:

    - $action

        Action to be checked.

    - $response

        Response from user:
        `'init'`, false value (not yet checked), `'confirm'` and others (cancelled).
        This may typically be given by user using `response_action` parameter.

    - arg => $arg

        Argument(s) of action.

    - previous\_action => $previous\_action

        The action users will be redirected when action is confirmed.
        This may typically given by user using `previous_action` parameter.

- is\_anonymous ( )

    _Instance method_.
    TBD.

- renew ( )

    _Instance method_.
    Renews the session.
    Updates internal session ID and HTTP cookie.

- store ( )

    _Instance method_.
    Stores session into session store.

## Functions

- check\_cookie\_extern ( )

    _Function_.
    Deprecated.

- decrypt\_session\_id ( )

    _Function_.
    Deprecated.

- encrypt\_session\_id ( )

    _Function_.
    Deprecated.

- list\_sessions ( )

    _Function_.
    TBD.

- purge\_old\_sessions ( )

    _Function_.
    Deprecated.

- set\_cookie ( $cookie\_domain, $expires, \[ $use\_ssl \] )

    _Instance method_.
    TBD.

- set\_cookie\_extern ( $cookie\_domain, \[ $use\_ssl \] )

    _Instance method_.
    Deprecated.

## Attributes

TBD.

# SEE ALSO

[Sympa::DatabaseManager](./Sympa-DatabaseManager.3.md).

# HISTORY

[SympaSession](https://metacpan.org/pod/SympaSession) appeared on Sympa 5.4a3.

It was renamed to [Sympa::Session](./Sympa-Session.3.md) on Sympa 6.2a.41,
then [Sympa::WWW::Session](./Sympa-WWW-Session.3.md) on Sympa 6.2.26.

["confirm\_action"](#confirm_action) method was added on Sympa 6.2.17.
