Message workflow
================

Does Sympa alter messages?
--------------------------

Sympa receives incoming messages in its `msg` spool. An image of the message is then created (a MIME::Entity, using MIME-Tools CPAN library) for later processing. At the end, the MIME::Entity object is dumped to the mail tranfer agent (sendmail, postfix,...). Note that the message might be altered by this intermediate MIME::Entity object (row size for Base64 encoded MIME parts for example). The use of the MIME::Entity intermediate object is skipped is a message is S/MIME signed because any changes in any part of the message body would break the S/MIME signature.

Note also that Sympa might perform the following changes to the distributed messages; however some of this alterations of the messages can be configured.

  - SMTP header fields are added/removed for loop prevention (see [loop_detection](/manual/customizing#loop_detection)), for RFC 2369 compliance (see [rfc2369_header_fields](/conf-parameters/part2#rfc2369_header_fields)) or through customizations (see [remove_headers](/conf-parameters/part2#remove_headers)).

  - The subject of the message might be changed to add a custom subject (see [custom_subject](/manual/parameters-sending#custom_subject)).

  - Headers and footers may be added. They are either added as separate MIME parts or within the message body, if it is of text type (see [message_header_and_footer](/manual/list-definition#message_header_and_footer))

  - Message content-type may be changed from multipart/alternative to text/plain or text/html if the user has selected the corresponding reception mode (see [multipartalternative](/manual/reception-mode#multipartalternative)).

Attachments
-----------

Sympa distribution process copes well with MIME messages, including those including attachments. However there are situations special processing for messages with attachments:

  - [max_size](/manual/parameters-others#max_size) : this parameter controls the maximum allowed size for a distributed message,

  - urlize : list members can select this reception mode to have attachments removed from the message and stored on the list server. Urlizization depends on the message size and the [urlize_min_size](/manual/conf-parameters/part2#urlize_min_size) parameter.

Loop prevention
===============

