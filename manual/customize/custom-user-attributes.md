Custom user attributes
======================

If the user description parameters available in Sympa don't suit your needs, you can define your own description attributes. These attributes can be used when moderating subscription or message moderation. They provide additional, useful informations, when making a decision.

Custom attributes definition
----------------------------

These attributes are defined in the list configuration by the [custom_attribute list parameter](/manual/parameters-others#custom_attribute).
You can define as many attributes as you like.

Custom attributes provisionning
-------------------------------

Custom attributes can be provionned using two mutually exclusive ways:

1.  manually by the user

2.  extracted from and LDAP or SQL repository.

How are the custom attributes values obtained from users?
---------------------------------------------------------

Users can provide the information expected by your custom attributes on two occasions :

  - when *subscribing* to the list through the web interface. After hitting the "subscribe" button, the user is presented a form, each field of which corresponds to a custom attribute.

<a href="/_detail/manual/sub_form.png?id=manual%3Acustomizing" class="media" title="manual:sub_form.png"><img src="/_media/manual/sub_form.png" class="media" /></a>

  - when *modifying their profile* through the web interface.

<a href="/_detail/manual/sub_options.png?id=manual%3Acustomizing" class="media" title="manual:sub_options.png"><img src="/_media/manual/sub_options.png" class="media" /></a>

How is it stored?
-----------------

The custom attributes are stored as XML fragments in the [subscriber_table table](/internals/index#subscriber_table). This fragment is located in the `custom_attribute_subscriber` field.

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

the custom attributes are displayed for each user in the subscribers review of the web interface.

You can use these attributes to [customize messages](/manual_6.1/message-handling#customizing_messages).

