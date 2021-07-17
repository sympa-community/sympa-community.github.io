---
title: 'Message personalization'
up: ../customize.md#customizing-sympa-services
---

Message personalization
=======================

How it works
------------

(Work in progress)

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

      - `message_footer` file was named "`message.footer`".
      - `message_header` file was named "`message.header`".

  * The line above is an example for Sympa 6.2 later than Aug 2016.
    With earlier versions it should be:
    ``` code
    Click here to unsubscribe: [% wwsympa_url %]/auto_signoff/[% listname %]/[% user.escaped_email %]
    ```

----

