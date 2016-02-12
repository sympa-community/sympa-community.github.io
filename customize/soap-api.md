Sympa SOAP server
=================

Introduction
============

(Work in progress)

Supported functions
===================

Note that all functions accessible through the SOAP interface apply the appropriate access control rules, given the user's privileges.

The following functions are currently available through the Sympa SOAP server :

  - login : user email and passwords are checked against Sympa user DB, or another backend.

  - casLogin : this function will verify CAS proxy tickets against the CAS server

  - authenticateAndRun : useful for SOAP clients that can't set an HTTP cookie ; they can provide both the Sympa session cookie and the requested command in a single call

  - authenticateRemoteAppAndRun : equivalent of the previous command used in a trusted context (see [trust_remote_applications](#trust_remote_applications))

  - lists : provides a list of available lists (authorization scenarios are applied)

  - complexLists : same as the previous feature, but provides a complex structure for each list

  - info : provides description informations about a given list

  - which : gets the list of subscription of a given user

  - complexWhich : same as previous command, but provides a complex structure for each list

  - amI : tells if a given user is member of a given list

  - review : lists the members of a given list

  - subscribe : subscribes the current user to a given list

  - signoff : current user is removed from a given list

  - add : used to add a given user to a given list (admin feature)

  - del : removes a given user from a given list (admin feature)

  - createList : creates a new mailing list (requires appropriate privileges)

  - closeList : closes a given mailing list (admin feature)

Note that when a list parameter is required for a function, you can either provide the list name or the list address. However the domain part of the address will be ignored.

Check [the_wsdl_service_description](#the_wsdl_service_description) for detailed API informations.

Web server setup
================

**Starting Sympa 5.4**, the sympa\_soap\_server is wrapped in small C script, sympa\_soap\_server-wrapper.fcgi, in order to avoid to use the -unsecure and no longer maintained - setuid perl mode.

(Work in progress)

Until version 5.3
-----------------

(Work in progress)

Version 5.4 and higher
----------------------

Here is a sample piece of your Apache `httpd.conf` with a SOAP server configured and using the C wrapper:

``` code
      FastCgiServer /home/sympa/bin/sympa_soap_server-wrapper.fcgi -processes 1
      ScriptAlias /sympasoap /home/sympa/bin/sympa_soap_server-wrapper.fcgi

      <Location /sympasoap>
           SetHandler fastcgi-script
      </Location>
```

Sympa setup
===========

(Work in progress)

Trust remote applications
=========================

(Work in progress)

Below is a sample Perl code that does a SOAP procedure call (for a SUBSCRIBE sympa command) using the trusted\_application feature :

``` code
my $soap = new SOAP::Lite();
$soap->uri('urn:sympasoap');
$soap->proxy('http://myserver/sympasoap');

my $response = $soap->authenticateRemoteAppAndRun('myTestApp', 'myTestAppPwd', 'USER_EMAIL=userProxy@my.server', 'subscribe', ['myList@dom']);
```

[S. Santoro](mailto:dereckson@espace-win.org) wrote its own [PHP Trusted Application library for Sympa](/contribs/index#php_soap_library).

The WSDL service description
============================

(Work in progress)

Client-side programming
=======================

(Work in progress)

Writing a Java client with Axis
-------------------------------

(Work in progress)

The test command line SOAP client
=================================

Sympa distribution includes a simple command line application that allows you to test SOAP request towards your Sympa SOAP server. This script is named sympa\_soap\_client.pl and is located in the Sympa bin directory.

The [four methods](/manual/soap#introduction) available through the Sympa SOAP server can be tested using this tool. There is no explicit option to tell what acces methos is used. It is inferred based on what options are provided to the script.

### Getting the email associated to a session id

You must use the id of a session actually used at the time you launch the command. It is the value of the “sympa\_session” cookie set when accessing to the Sympa web interface.

#### Command line

``` code
# /home/sympa/bin/sympa_soap_client.pl
 --soap_url=<SOAP server URL>
 --cookie=<cookie identifier>
```

  - --soap\_url: the URL to your Sympa SOAP server

  - --cookie: the value of the “sympa\_session” cookie set when accessing to the Sympa web interface.

#### Expected output

``` code
error : get_email_cookie
cookie : 65354224256806


getEmailUserByCookie....
0
        'mail@renater.fr'
```

### Using the Sympa SOAP functions with the command line tool

It is done by calling the script and providing two kind of arguments :

  - the argument required by the service usage : SOAP URL, service name and service parameters,

  - the arguments allowing to authenticate the user requesting the service.

#### Authentication using an HTTP session cookie

Actually, providing the HTTP cookie to a command line sums up in providing a session id, i.e. a simple number. You must use the value of a session cookie actually used at the time you launch the command. It is the “sympa\_session” cookie set when accessing to the Sympa web interface.

``` code
# /home/sympa/bin/sympa_soap_client.pl --soap_url=<SOAP server URL>
                                       --service=<a sympa service>
                                       --service_parameters=<value1,value2,value3>
                                       --session_id=<cookie identifier>
```

The options used are:

  - --soap\_url: the URL to your Sympa SOAP server;

  - --service: the requested SOAP service. See [below](#sympa_soap_services_and_the_command_line_tool);

  - --service\_parameters: the parameters needed to use the service. They must be provided as a comma separated list, without spaces. See [below](#sympa_soap_services_and_the_command_line_tool);

  - --session\_id: the value of the “sympa\_session” cookie set when accessing to the Sympa web interface.

#### Authentication using a user name and password

``` code
# /home/sympa/bin/sympa_soap_client.pl --soap_url=<SOAP server URL>
                                       --service=<a sympa service>
                                       --service_parameters=<value1,value2,value3>
                                       --user_email=<email>
                                       --user_password=<password>
```

The options used are:

  - --soap\_url: the URL to your Sympa SOAP server;

  - --service: the requested SOAP service. See [below](#sympa_soap_services_and_the_command_line_tool);

  - --service\_parameters: the parameters needed to use the service. They must be provided as a comma separated list, without spaces. See [below](#sympa_soap_services_and_the_command_line_tool);

  - --user\_email: the email of the user requesting the service;

  - --user\_password: the password of this user.

#### Access through a trusted application

``` code
# /home/sympa/bin/sympa_soap_client.pl --soap_url=<SOAP server URL>
                                       --service=<a sympa service>
                                       --service_parameters=<value1,value2,value3>
                                       --cookie=<cookie identifier>
                                       --trusted_application=<app name>
                                       --trusted_application_password=<password>
                                       --proxy_vars=<id=value,id2=value2>
```

The options used are:

  - --soap\_url: the URL to your Sympa SOAP server;

  - --service: the requested SOAP service. See [below](#sympa_soap_services_and_the_command_line_tool);

  - --service\_parameters: the parameters needed to use the service. They must be provided as a comma separated list, without spaces. See [below](#sympa_soap_services_and_the_command_line_tool);

  - --cookie: the value of the “sympa\_session” cookie set when accessing to the Sympa web interface;

  - --trusted\_application: the trusted application name as defined in `trusted_applications.conf`;

  - --trusted\_application\_password: the password of the trusted application as defined in `trusted_applications.conf`;

  - --proxy\_vars: the proxy vars of the trusted application as defined in `trusted_applications.conf`. This is a comma-separated list of values. For example, if you have defined in `trusted_applications.conf` the following variables: `proxy_for_variables USER_EMAIL,remote_host`, then you will use it this way in the proxy\_vars option: `--proxy_vars=USER_EMAIL=user.email@domain.tld,remote_host=remote.host.domain.tld`.

### Sympa SOAP services and the command line tool

This is a description of how to use the Sympa SOAP services using the command line tool. The parameters are given in the same order they must be found in the command tool option `service_parameters`. They must be provided as a comma separated list, without spaces. Don't forget to escape characters that would break the command line, such as spaces, exclamation marks and so on.

********
Parameters example

If the list of parameters is:

  - list name

  - user email

Then the `service_parameters` option will look like:

--service\_parameters=mylist,mail@renater.fr

********

#### login

No object here: this is the service used to log when the command tool uses a username and password.

#### casLogin

No object here.

#### authenticateAndRun

No object here: this the service used by the command line tool to call the other services, when authentication is done through session id or user name + password.

#### authenticateRemoteAppAndRun

No object here: this the service used by the command line tool to call the other services, when testing trusted applications.

#### lists

The parameters are optional.

Parameters:

  - topic: the topic of the lists to return

  - subtopic: the subtopic of this topic

Output example:

``` code
lists....
0
        'homepage=http://domain.tld/sympa/info/amietestdv01;subject=Amical;listAddress=amietestdv01@domain.tld'
1
        'homepage=http://domain.tld/sympa/info/archeologie;subject=Liste sur l'archéologie;listAddress=archeologie@domain.tld'
2
        'homepage=http://domain.tld/sympa/info/blackmambo;subject=A black mambo;listAddress=blackmambo@domain.tld'
3
        'homepage=http://domain.tld/sympa/info/bluemambo;subject=Another mambo. This one is blue.;listAddress=bluemambo@domain.tld'
```

#### complexLists

The parameters are optional.

Parameters:

  - topic: the topic of the lists to return

  - subtopic: the subtopic of this topic

Output example:

``` code
AuthenticateAndRun complexLists....
0
        _homepage_
                'http://domain.tld/sympa-dv/info/amietestdv01'
        _listAddress_
                'amietestdv01@domain.tld'
        _subject_
                'Amical'
1
        _homepage_
                'http://domain.tld/sympa-dv/info/archeologie'
        _listAddress_
                'archeologie@domain.tld'
        _subject_
                'List sur l'archéologie'
2
        _homepage_
                'http://domain.tld/sympa-dv/info/blackmambo'
        _listAddress_
                'blackmambo@domain.tld'
        _subject_
                'A black mambo'
3
        _homepage_
                'http://domain.tld/sympa-dv/info/bluemambo'
        _listAddress_
                'bluemambo@domain.tld'
        _subject_
                'Another mambo. This one is blue.'
```

#### info

Parameters:

  - listname (mandatory): the name of the list for which info are requested

Output example:

``` code
```

#### which

All arguments are mandatory (at least with an empty value).

Parameters:

  - no parameters

Output example:

``` code
which....
0
        'isOwner=1;homepage=http://domain.tld/sympa/info/amietestdv01;subject=Amical;listAddress=amietestdv01@domain.tld;isEditor=0;isSubscriber=0'
1
        'isOwner=1;homepage=http://domain.tld/sympa/info/archeologie;subject=Liste sur l'archéologie;listAddress=archeologie@domain.tld;isEditor=0;isSubscriber=0'
2
        'isOwner=1;homepage=http://domain.tld/sympa/info/blackmambo;subject=A black mambo;listAddress=blackmambo@domain.tld;isEditor=0;isSubscriber=0'
3
        'isOwner=1;homepage=http://domain.tld/sympa/info/bluemambo;subject=Another mambo. This one is blue.;listAddress=bluemambo@domain.tld;isEditor=0;isSubscriber=0'
```

#### complexWhich

All arguments are mandatory (at least with an empty value).

Parameters:

  - no parameters

Output example:

``` code
complexWhich....
0
        _homepage_
                'http://dev-sympa.renater.fr/sympa-dv/info/redmambo'
        _isEditor_
                '0'
        _isOwner_
                '1'
        _isSubscriber_
                '0'
        _listAddress_
                'redmambo@dev-sympa.renater.fr'
        _subject_
                'Amical'
1
        _homepage_
                'http://dev-sympa.renater.fr/sympa-dv/info/bluemambo'
        _isEditor_
                '1'
        _isOwner_
                '1'
        _isSubscriber_
                '0'
        _listAddress_
                'bluemambo@dev-sympa.renater.fr'
        _subject_
                'Another mambo. This one is blue.'
2
        _homepage_
                'http://dev-sympa.renater.fr/sympa-dv/info/archeologie'
        _isEditor_
                '1'
        _isOwner_
                '1'
        _isSubscriber_
                '0'
        _listAddress_
                'archeologie@dev-sympa.renater.fr'
        _subject_
                'Liste sur l'archéologie'
3
        _homepage_
                'http://dev-sympa.renater.fr/sympa-dv/info/blackmambo'
        _isEditor_
                '0'
        _isOwner_
                '1'
        _isSubscriber_
                '0'
        _listAddress_
                'blackmambo@dev-sympa.renater.fr'
        _subject_
                'A black mambo'
```

#### amI

Parameters:

  - list name (mandatory): the name of the list for which the function is tested;

  - function (mandatory): the function the existence of which we will test. The allowed values are: `subscriber`, `owner` and `editor`;

  - user (mandatory): the email address of the user for whom we want to know if she has the function indicated in the target list.

Output example:

``` code
param: blackmambo
param: owner
param: david.verdin@renater.fr
Using Session_id 48339436597794


AuthenticateAndRun amI....
0
        '1'
```

#### review

Parameters:

  - the name of the list for which we want the subscribers list (mandatory).

Output example:

``` code
review....
0
        'mail1@renater.fr'
1
        'mail2@renater.fr'
2
        'mail3@renater.fr'
```

#### subscribe

Parameters:

  - list name (mandatory)

Output example:

``` code
subscribe....
0
        '1'
```

#### signoff

Parameters:

  - list name (mandatory)

Output example:

``` code
signoff....
0
        '1'
```

#### add

Parameters:

  - listname (mandatory): the name of the list we want to subscribe the mail address to;

  - email (mandatory): the email to subscribe to the list;

  - gecos: the name under which this email will be subscribed (for example: “John Doe”);

  - quiet: if set to '1', the user doesn't receive a subscription notification

Output example:

``` code
add....
0
        ''
```

#### del

Parameters:

  - listname (mandatory): the name of the list we want to unsubscribe the mail address from;

  - email (mandatory): the email of the user to unsubscribe;

  - quiet: if set to '1', the user doesn't receive an unsubscription notification

Output example:

``` code
del....
0
        '1'
```

#### createList

Parameters:

  - the list name (mandatory);

  - the subject of the list (mandatory);

  - the template to use (mandatory) (the name of a template found in the `create_list_templates` directory for this Sympa robot;

  - the description of the list (mandatory);

  - the topic of the list (mandatory) (one among the different options existing in topics.conf).

Output example:

``` code
param: orangemambo
param: Dude !
param: hotline
param: La liste verte
param: computing
Using Session_id 4860001445687


AuthenticateAndRun createList....
0
        '1'
```

#### closeList

Parameters:

  - the name of the list to close (mandatory).

Output example:

``` code
param: orangemambo
Using Session_id 4860001445687


AuthenticateAndRun closeList....
0
        '1'
```
