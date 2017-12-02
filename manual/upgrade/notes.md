---
title: 'Upgrading to latest version of Sympa'
up: ../upgrade.md
---

Upgrading to latest version of Sympa
====================================

----
Note:

  * See also "[Upgrading Sympa](../upgrade.md)" for general upgrading
    instruction.

----

Upgrading from earlier Sympa 6.2.x
----------------------------------

After release of 6.2, several changes on templates are made. Especially on
6.2.18, web templates for shared document repository and list configuration
edit form broke backward compatibility in exchange for bug fixes.

If you have customized templates with earlier version, you should check if web
interface will work correctly after upgrading, and reapply customization to new
templates as necessity.

Upgrading from Sympa 6.1.x or earlier
-------------------------------------

Several changes in Sympa 6.2 require to perform specific operations when
upgrading. Here is the ordered list of operations to perform.

  1. [_Source distribution only_] Check the options for ``configure`` script.

     As some Sympa binaries have been renamed, your sympa init script have to
     be updated. Please make sure your configure options have been correctly
     set, so that this script ends up in the correct location.

     See "[Run configure script](../install/install-sympa-distribution-source.md#run-configure-script)"
     for details.

  2. [_Source distribution only_] Run additional commands after
     ``make install``:
     ``` bash
     # sympa_wizard.pl --check            # Update dependent modules
     # sympa.pl --upgrade_config_location # move existing configs to /etc/sympa/ directory
     # sympa.pl --upgrade                 # merge sympa.conf and wwsympa.conf
     ```

  3. Commands after upgrade:

     These are always needed you are using either binary distribution or source
     distribution.
     ``` bash
     # upgrade_bulk_spool.pl              # move messages stored in database to filesystem
     # upgrade_send_spool.pl              # move messages sent through web interface to the new format
     ```
     For details, read manual pages of
     [upgrade_bulk_spool.pl](../man/upgrade_bulk_spool.1.md) and
     [upgrade_send_spool.pl](../man/upgrade_send_spool.1.md).

  4. For all your **custom action** templates, move them into
     **[``$SYSCONFDIR``](../layout.md#sysconfdir)`/custom_actions`**
     directory.

After the process above, the following changes will have occured:

  - The `sympa.conf` is now located in a `sympa/` subdirectory.
  - Content of ``wwsympa.conf`` will have been merged into `sympa.conf`.
    `wwsympa.conf` will no longer be used.
  - The `sympa.conf` remaining will have been stripped of any "`color_*`"
    parameters, to prevent you from having a messed up web interface.

      - Any **`robot.conf`** will have had the same treatment as `sympa.conf`
        (copy, then stripped of `color_*` parameters).

  - Older `sympa.conf` will have need copied to
    "`sympa.conf.upgrade.`_dd_`.`_Month_`.`_yyyy_`-`_hh_`.`_mm_`.`_ss_".
  - Any customized "**`web_tt2`**" directory (either in
    [``$SYSCONFDIR``](../layout.md#sysconfdir)`/` or in
    [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`_robot_`/`)
    will have been renamed to
    "`web_tt2.upgrade.`_dd_`.`_Month_`.`_yyyy_`-`_hh_`.`_mm_`.`_ss_".

    Indeed, as most of the Sympa templates have been changed for the new skin,
    your customizations could not be compliant to the new web interface. You
    should see how to reintroduce them to the new templates.

