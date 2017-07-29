Start mailing list service
==========================

1. Ensure that RDBMS is running (on SQLite, service need not to be run).

2. Ensure that MTA and HTTP server are running.

3. Start Sympa service.

   If you are using nginx, you must run WWSympa service, too.

   If you are using nginx and wish to use SOAP service, you must run SympaSOAP serivce, too.

   * If your system supports Systemd:
     ```
     # systemctl start sympa.service
     ```
     and optionally,
     ```
     # systemctl start wwsympa.service
     # systemctl start sympasoap.service
     ```

   * If your system supports initscripts:
     ```
     # service sympa start
     ```
     and optionally,
     ```
     # service wwsympa start
     # service sympasoap start
     ```

4. Start Web browser and access to <http://your.host.dom.ain/sympa>.

5. Follow ``First login`` link, input listmaster address chosen on the
   first section, and then click ``Send my password`` button.

6. E-mail describing how to choose your password (are you a listmaster,
   aren't you?) will be sent.  According to description, choose your
   password.

