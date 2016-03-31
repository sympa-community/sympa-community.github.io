Modlist / Whitelist plugin
==========================

Whitelist/Modlist Extension for Sympa 6.2

Steve Shipway, University of Auckland, 2012 - 2015

This adds Whitelist and Modlist functionality to Sympa, by using an included scenario, and a custom action module to hold the required Perl code.

Download
--------

You can retrieve this plugin from [Steve Shipway's web site](http://steveshipway.org/software/f_sympa.html).

Installing
----------

----
Important:

  * Your installation may use different directories to hold the various files. Change the paths to what is appropriate for your system.

----

  - Copy whitelist.pm and modlist.pm to the custom\_actions directory. These are the custom actions. If you only copy the whitelist.pm then modlist functionality will be disabled. Either put them at the top level, or at robot level as you prefer.

  - Create default empty whitelist.txt and modlist.txt files in search\_filters (or wherever your Sympa search\_filters path is). These must exist as a default for lists that do not have a defined whitelist or modlist.

  - Install the whitelist.tt2 template into the web\_tt2 directory. This is the admin pages for the whitelist and modlist. It goes into your web\_tt2 customisation directory.

  - Update nav.tt2 on your system. This is where you add the new Whitelist and Modlist menu items. The supplied nav.tt2 file should work with Sympa 6.2.x but if you have already customised nav.tt2 you should make sure to add the necessary Whitelist and Modlist parts.

  - Update search.tt2 and review.tt2. These add the Whitelist and Modlist buttons to the subscribers review page. The supplied files should work with Sympa 6.2.x but if you have already customised them you should make sure to add the necessary Whitelist and Modlist parts.

  - Update admin.t22. This adds the white/modlist options to the list admin page. This is optional but recommended.

  - Copy include.send.header into your scenari directory. This activates the Whitelist and Modlist on all lists, though until you define some entries, all lists will get the default empty files you set up in step 2.

  - Restart the Sympa daemons, and restart your web server. This will pick up the changes.

  - TEST! Choose a list and verify that the Whitelist and Modlist tabs appear in the admin page. Try adding an entry to the whitelist and verify that it works and produces no errors. Make sure the tabs appear for Whitelist and Modlist in the web interface for list owners and admins.

Caveats
-------

The new white/modlist admin pages are in English and do not have translations. You might be able to update them yourself to help with this.

Make sure file ownerships are correct.

The future
----------

There will be the ability to add buttons to the Modindex page to allow items to be added to the whitelist at the click of a button.
