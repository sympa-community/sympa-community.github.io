Automating Startup
==================

Systemd
-------

  * Units should be customized to ensure Sympa starts after database server
    (if you run it except SQLite on the same host as Sympa).  It can be done
    by editing configuration file:

      /etc/systemd/system/sympa.service.d/dependencies.conf

    Note that database service should also start before HTTP service.

  * Run ``systemctl enable sympa.service`` to activate sympa service.

    * If you are using nginx, run ``systemctl enable wwsympa.service`` too.

    * If you are using nginx and wish to use SOAP service, run
      ``systemctl enable sympasoap.service`` too.

initscripts
-----------

  * Run ``chkconfig sympa on`` to activate sympa service.

    * If you are using nginx, run ``chkconfig wwsympa on`` too.

    * If you are using nginx and wish to use SOAP service, run
      ``chkconfig sympasoap on`` too.

