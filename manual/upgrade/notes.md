`   `

Upgrading to Sympa 6.2
======================

Several changes in Sympa 6.2 require to perform specific operations when upgrading. Here is the ordered list of operations to perform.

The changes are the following:

  - sympa.conf and wwsympa.conf are merged. wwsympa.conf is no longer used,

  - sympa.conf is now located in a sympa/ sub-directory,

  - bulk spool is moved from database to file system,

  - formalism for storing messages sent from the web interface has changed

  - custom actions must now be located in a dedicated `custom_actions/` directory,

  - Sympa skin was changed.

  - init script is changed, becausee some Sympa daemons have been renamed. Check your configure optins to make sure the init script ends up in the correct location for your operating system.

Here is the list of commands to run:

``` code
sympa_wizard.pl --check # Update CPAN dependencies
sympa.pl --upgrade_config_location # move existing configs to /etc/sympa/ directory
sympa.pl --upgrade # merge sympa.conf and wwsyamp.conf
upgrade_bulk_spool.pl # move messages stored in database to filesystem
upgrade_send_spool.pl # move messages sent through web interface to the new formalism.
```

\* For all your **custom action** templates, move them to **etc/custom\_actions** directory.
  - **After the upgrade, the following changes will have occured:**

      - **sympa.conf** will have need copied to sympa.conf.upgrade.&lt;dd&gt;.&lt;Month&gt;.&lt;yyyy&gt;-&lt;hh&gt;.&lt;mm&gt;.&lt;ss&gt; The sympa.conf remaining will have been stripped of any 'color\_*' parameters, to prevent you from having a messed up web interface \* any **robot.conf** will have had the same treatment as sympa.conf (copy, then stripped of color\_* parameters).

      - any customized "**web\_*t2**" directory (either in etc/ or in etc/&lt;robot&gt;/) will have been renamed "**web\_*tt2.upgrade.&lt;dd&gt;.&lt;Month&gt;.&lt;yyyy&gt;-&lt;hh&gt;.&lt;mm&gt;.&lt;ss&gt;**". Indeed, as most of the Sympa templates have been changed for the new skin, your customizations could not be compliant to the new web interface. You should see how to reintroduce them to the new templates.

      - Finally, as some Sympa binaries have been renamed, your sympa init script will have been updated. Please make sure your configure options have been correctly set, so that this script ends up in the correct location.


