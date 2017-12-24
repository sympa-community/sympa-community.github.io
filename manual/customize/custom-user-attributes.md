---
title: 'Custom user attributes'
up: ../customize.md#customizing-sympa-services
---

Custom user attributes
======================

If the user description parameters available in Sympa don't suit your needs, you can define your own description attributes. These attributes can be used when moderating subscription or message moderation. They provide additional, useful informations, when making a decision.

Custom attributes definition
----------------------------

These attributes are defined in the list configuration by the [`custom_attribute`](../man/list_config.5.md#custom_attribute) parameter.
You can define as many attributes as you like.

Custom attributes provisioning
------------------------------

Custom attributes can be provionned using two mutually exclusive ways:

  1. manually by the user
  2. extracted from and LDAP or SQL repository
     (see [`include_ldap_ca`](../man/list_config.5.md#include_ldap_ca)
     and [`include_sql_ca`](../man/list_config.5.md#include_sql_ca)
     list configuration parameters).

How are the custom attributes values obtained from users?
---------------------------------------------------------

Users can provide the information expected by your custom attributes on two occasions :

  - when *subscribing* to the list through the web interface. After hitting the "subscribe" button, the user is presented a form, each field of which corresponds to a custom attribute.

----
[sub_form.png: Work in progress]

----

  - when *modifying their profile* through the web interface.

----
[sub_options.png: Work in progress]

----

How is it stored?
-----------------

The custom attributes are stored as XML fragments in the
[`subscriber_table`](../man/sympa_database.5.md#subscriber_table) table of
database. This fragment is located in the `custom_attribute_subscriber` field.

Here is an example of such an XML fragment, which contains two custom attributes :

  - the first one has the id "accr" and has the value "ultra-violet";
  - the second one has thee id "pt" and has the value "0".

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<custom_attributes>
  <custom_attribute id="accr">
    <value> ultra-violet</value>
  </custom_attribute>
  <custom_attribute id="pt">
    <value>0</value>
  </custom_attribute>
</custom_attributes>
```

So, what can you do with that feature?
--------------------------------------

The custom attributes are displayed for each user in the subscribers review of the web interface.

You can use these attributes to ~~[message personalization](message-personalization.md)~~.

