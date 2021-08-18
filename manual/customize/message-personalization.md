---
title: 'Message personalization'
up: ../customize.md#customizing-sympa-services
---

Message personalization
=======================

How it works
------------

(Work in progress)

List of template variables
--------------------------

Following TT2 template variables in the message body and/or
message footer/header will be extracted.

  - `[% listname %]`
    Name of the list.

  - `[% domain %]`
    Mail domain name of the list.

    ----
    Note:

      * On Sympa prior to 6.2.54, `[% robot %]` may be used.

    ----

  - `[% wwsympa_url %]`
    URL prefix of web interface, if enabled.

  - `[% user %]`
    A hash containing the information of the subscriber.

      - `[% user.email %]`
        E-mail address.

      - `[% user.gecos %]`
        Display name.

      - `[% user.bounce %]`
      - `[% user.bounce_address %]`
      - `[% user.bounce_score %]`
        Bounce information, if the subscriber is bouncing.

      - `[% user.inclusion %]`
      - `[% user.inclusion_ext %]`
      - `[% user.inclusion_label %]`
        Information of inclusion (Sympa 6.2.45b or later).

        ----
        Note:
          - On Sympa 6.2.44 or earlier, use `[% user.inclusion %]`
            that has true value if the subscriber is included.

        ----

      - `[% user.subscribed %]`
        True value if the subscriber has been added manually.

      - `[% user.suspend %]`
      - `[% user.startdate %]`
      - `[% user.enddate %]`
        Information about suspension, if the subscriber is suspended.

      - `[% user.reception %]`
        The reception mode.  See also
        "[Message reception modes](https://sympa-community.github.io/manual/customize/basics-alterations.html#message-reception-modes)".

      - `[% user.topics %]`
        Selected message topics.  See also "[Message topics](https://sympa-community.github.io/manual/customize/basics-delivery.html#message-topics)".

      - `[% user.visibiity %]`
        Visibility mode, i.e. whether the subscriber is listed in the list
        review page or not.  `conceal` or `noconceal`.

      - `[% user.date %]`
        Unix time of addition.

      - `[% user.update_date %]`
        Unix time of the last update.

      - `[% user.number_messages %]`
        Number of messages this subscriber has received through this list.

      - `[% user.custom_attribute.<id>.value %]`
        Custom attribute if any.  See also
        "[Custom user attributes](https://sympa-community.github.io/manual/customize/custom-user-attributes.html)".

### Obsoleted template variables

These variables were obsoleted.

  - `[% user.escaped_email %]`
    Preferred: `[% user.email | uri %]`

  - `[% user.friendly_date %]`
    Preferred: `[% user.date | optdesc('unixtime') %]`


Adding unsubscription links
---------------------------

This is an example of an application of message personalization feature.

Add a file named `message_footer` to the list directory with following
content:

``` code
Click here to unsubscribe: [% 'auto_signoff' | url_abs([listname],{email=>user.email}) %]
```

And set these list parameters:

  * "Allow message personalization"
    ([`personalization_feature`](/gpldoc/man/sympa_config.5.html#personalization_feature)
    parameter) should be "enabled" (`on`).
  * "Message personalization" - "Scope for messages from incoming email"
    ([`personalization.mail_apply_on`](/gpldoc/man/sympa_config.5.html#personalizationmail_apply_on)
    parameter) should be "header and footer" (`footer`).

Then, each time a message is sent to the list, this file will be converted
and allow to display the following text at the bottom of each message:

``` code
Click here to unsubscribe: https://web.example.org/sympa/auto_signoff/mylist?email=bob.mcbob%40domain.tld
```

----
Notes:

  * On Sympa earlier than 6.2.60:

      - `personalization_feature` parameter was named "`merge_feature`".
      - `personalization.mail_apply_on` was not required (not implemented).

  * On Sympa earlier than 6.2.42:

      - `message_footer` file was named "`message.footer`", and so on.
        See the notes in
        "[Message header and footer](../customize/basics-list-config.md#message-header-and-footer)".

  * The line above is an example for Sympa 6.2 later than Aug 2016.
    With earlier versions it should be:
    ``` code
    Click here to unsubscribe: [% wwsympa_url %]/auto_signoff/[% listname %]/[% user.escaped_email %]
    ```

----

