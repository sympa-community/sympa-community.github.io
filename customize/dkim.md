DKIM features for Sympa
=======================

----
Important:

* DKIM as been introduced in Sympa in version **6.1**.

----

DKIM is a cryto signature method designed to prevent spam, phishing etc. As postmaster or listmaster you should consider both the question of taking advantage of DKIM status of each incoming message and the question of signing all or a subset of outgoing messages.

Incomming messages
------------------

DKIM status of incomming messages should be used by the MTA serving Sympa domain in order to reject some messages. This is not targeted by this section.

However, the mailing list server can take advantage of existing DKIM signature in order to increase the trust of the message while evaluating message workflow. This is based on [scenario mechanism](/manual/authorization-scenarios). A authentication level named `dkim` can be used, it is smilar to the one named `smtp` but it will be applied only for message with a valid signature (dkim signature status = pass).

In order to activate such feature, you should install Mail::DKIM cpan module (and it's dependancies) and turn on dkim feature by editing sympa.conf (or robot.conf) to set `on` the parameter [dkim_feature](/conf-parameters/part3).
