Start mailing list service
==========================

Systemd
-------

  * Ensure that RDBMS is running (on SQLite, service need not to be run).

  * Ensure that MTA and HTTP server are running.

  * Run ``systemctl start sympa.service``.

    * If you are using nginx, you must run ``systemctl start wwsympa.service``
      too.

    * If you are using nginx and wish to use SOAP service, you must run
      ``systemctl start sympasoap.service`` too.

  * Start Web browser and access to <http://your.host.dom.ain/sympa>.

  * Follow ``First login`` link, input listmaster address chosen on the
    first section, and then click ``Send my password`` button.

  * E-mail describing how to choose your password (are you a listmaster,
    aren't you?) will be sent.  According to description, choose your
    password.

initscripts
-----------

  * Ensure that RDBMS is running (on SQLite, service need not to be run).

  * Ensure that MTA and HTTP server are running.

  * Run ``service sympa start``.

    * If you are using nginx, you must run ``service wwsympa start`` too.

    * If you are using nginx and wish to use SOAP service, you must run
      ``service sympasoap start`` too.

  * Start Web browser and access to <http://your.host.dom.ain/sympa>.

  * Follow ``First login`` link, input listmaster address chosen on the
    first section, and then click ``Send my password`` button.

  * E-mail describing how to choose your password (are you a listmaster,
    aren't you?) will be sent.  According to description, choose your
    password.

