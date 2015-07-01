Custom actions
==============

Starting Sympa 6.1, you can create your own actions, i.e. you can display any [TT2 template](http://template-toolkit.org/docs/manual/index.html) in the Sympa web interface. These templates will be processed and completely integrated to Sympa, using its CSS and the data from the server.

Custom actions are used to run specific code and/or display user defined templates. They can be executed in list or global context (it is up to you to decide what to do in both cases). Previously, a custom action was a simple TT2 template added to the web interface. It could only display data, not process them. They were improved to allow greater expressiveness.

**Warning:** AFTER UPGRADING TO 6.2, ANY PRE-EXISTING CUSTOM ACTION MUST BE MOVED TO THE RELEVANT CUSTOM\_ACTION DIRECTORY TO KEEP WORKING.

You can create a *your_action*`.pm` module under `etc/custom_actions` or `etc/`*robot*`/custom_actions` (This module must define a package called *your_action*`_plugin`) with a single `process()` sub to add custom processing. In list context you can also create it under `expl(/`*robot*`)?/`*list*`/custom_actions`. You can also create a *your_action*`.tt2` file at the same place to display your template. You don't need the `<head>` section or the `<body>` tag.

Your custom action is reachable using URL:

  - [http://your-sympa-server-root-url/ca/your_action/param1/param2/param3/...](http://your-sympa-server-root-url/ca/your_action/param1/param2/param3/...) in global context

  - [http://your-sympa-server-root-url/lca/your_action/listname/param1/param2/param3/...](http://your-sympa-server-root-url/lca/your_action/listname/param1/param2/param3/...) in list context

`param1`, `param2`, etc. are parameters that can be later used by the custom action.

The HTML code in *your_action*`.tt2` can make use of the parameters this way: \[% cap.1 %\] for the first parameter (param1 in the example URL above), \[% cap.2 %\] for the second one, and so on. If the module is not defined the template is simply displayed.

You can even have a robot-common *your_action*`.pm` module with a specific *your_action*`.tt2` for each robot as the file (.pm or .tt2) is conducted in this order :

  - expl/*robot*/*list*/custom\_actions (if list context and robot support)

  - expl/*list*/custom\_actions (if list context and no robot support)

  - etc/*robot*/custom\_actions (if robot support)

  - etc/custom\_actions

The module `process()` sub receives @cap entries as arguments (the List object is prepended to the argument list in list context). The module `process()` sub return value can be either:

  - 1, to parse and display the custom action related tt2

  - *a global action name*, to display its template

  - ca:*other_custom_action*, to parse and display another custom action related tt2 - you can therefore do your own custom workflow inside Sympa.

  - a hash, which entries will override $param ones, in case "custom\_action" or "next\_action" are present they act as described above.

Example : (etc/custom\_actions/foo.pm)

``` perl
package foo_plugin;
 
sub process {
    my $arg = shift;
    return 'info' if(ref($arg) eq 'List'); # Show list info page if in list context
    return 'ca:bar' if($arg eq 'bar'); # Show bar custom action tt2 if first arg is "bar"
    return {name => 'John'} if($arg eq 'barbar'); # Show foo.tt2 (own tt2) after putting "John" under the key "name" in $param
    return {name => 'John', next_action => 'review'} if($arg eq 'barbarbar'); # Same thing, but showing review page
    return 1; # Just showing own tt2 (foo.tt2)
}
1;
```

``` html4strict
<h2>A test action</h2>
[% IF list %]
<p>liste: [% list %]</p>
[% END %]
<p>Custom action name: [% custom_action %]</p>
<p>parameters:
<ol>
[% FOREACH param=cap %]
<li><b>[% param %]</b></li>
[% END%]
</ol>
</p>
```

Here is the result:

----
[custom_action.png: Work in progress]

----

