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

  - authenticateAndRun : usefull for SOAP clients that can't set an HTTP cookie ; they can provide both the Sympa session cookie and the requested command in a single call

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

Check [the_wsdl_service_description](#the_wsdl_service_description) for detailed API informations.

Web server setup
================

(Work in progress)

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

The WSDL service description
============================

(Work in progress)

Client-side programming
=======================

(Work in progress)

Writing a Java client with Axis
-------------------------------

(Work in progress)
