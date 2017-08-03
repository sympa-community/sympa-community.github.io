Install dependent modules
=========================

Binary distributions
--------------------

If you installed binary distribution (apt, RPM, ports, ...), required
dependent modules may have been installed: You can
[skip this section](generate-initial-configuration.md).

General instruction
-------------------

Run ``sympa_wizard`` to install dependent modules.
```
# sympa_wizard.pl --check
```
It checks your system, gets lacking modules from [CPAN](https://www.cpan.org/) and installs them.

Also, you can use any package management tools on your system (apt, yum, pkg, ...) or generic tools ([cpan](http://perldoc.perl.org/cpan.html), [cpanm](https://metacpan.org/pod/distribution/App-cpanminus/bin/cpanm), ...).

To know what modules you should install,
see [``$MODULEDIR/Sympa/ModDef.pm``](../man/Sympa-ModDef.3.md).

