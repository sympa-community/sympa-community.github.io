---
title: 'Custom scenario conditions'
up: ../customize.md#extending-sympa
---

Custom scenario conditions
==========================

You can use a Perl package of your own to evaluate a custom condition in scenario. It can be useful if you have very complex tasks to carry out to evaluate your condition (web services queries...). In this case, you should write a Perl module, place it in the `CustomCondition` name space, with one verify fonction that has to return `1` to grant access, `undef` to warn of an error, or anything else to refuse the authorization.

This Perl module:

  - must be placed in a subdirectoy `custom_conditions` of [``$SYSCONFDIR``](../layout.md#sysconfdir) directory or [``$SYSCONFDIR``](../layout.md#sysconfdir)`/`_mail domain_`/`.

  - its filename must be lowercase;

  - must be placed in the `CustomCondition` namespace;

  - must contain one `verify` static fonction;

  - will receive all condition arguments as parameters.

For example, lets write the smallest custom condition that always returns `1`.

[``$SYSCONFDIR``](../layout.md#sysconfdir)`/custom_conditions/yes.pm`:

``` perl
#!/usr/bin/perl

package CustomCondition::yes;

use strict;

# optional: We log parameters.
use Sympa::Log;
my $log = Sympa::Log->instance;

sub verify {
    my @args = @_;
    foreach my $arg (@args) {
        $log->syslog('debug3', 'arg: %s', $arg);
    }

    # I always say 'yes'.
    return 1;
}

# Packages must return true.
1;
```

We can use this custom condition that way:

``` code
CustomCondition::yes(,,)      smtp,smime,md5    -> do_it
true()                        smtp,smime        -> reject
```

Note that the `,,` are optional, but it is the way you can pass information to your package. Our `yes.pm` will print the values in the logs.

Remember that the package name has to be lowercase, but the `CustomCondition` namespace is case sensitive. If your package returns `undef`, the sender will receive an 'internal error' mail. If it returns anything else but `1`, the sender will receive a 'forbidden' error.

Tutorail: How to use message related variables within scenario rule conditions
------------------------------------------------------------------------------

This tutorial was *also* submitted by Thomas Berry, JPL, NASA. Thanks to him!

Scenario condition, message vars include:

``` code
[msg_body]
[msg_part->body]
```

When creating scenario rules that evaluate message content, two rules must be created when passing the contents of a message to a condition: one rule for plain text messages (`msg_body`) and a second rule for multi-part MIME messages (`msg_part`).

For example, I wrote a CustomCondition module that parses the URLs in a message and compares them with a list of approved URLs:

`send.url_eval`:
``` code
title Moderated with URL verification
CustomCondition::urlreview([listname],[sender],[msg_body]) smtp,smime,md5 -> reject()
CustomCondition::urlreview([listname],[sender],[msg_part->body]) smtp,smime,md5 -> reject()
true() smtp,smime,md5 -> editorkey
```

The CustomCondition module returns undef if the message contents are not provided by the rule. If the message contents are multi-part MIME, then the `[msg_body]` passed by the first rule is undefined, and the CustomCondition module returns undef causing the scenario to move on to the next rule which passes the parts of the multi-part MIME message to the module.

As for the CustomCondition module, it was written to evaluate both plain text (SCALAR) and multi-part MIME (ARRAY reference) being passed to the condition:

`urlreview.pm`:

``` perl
package CustomCondition::urlreview;

...

sub verify {
    my $listname = shift or return;
    my $sender   = shift or return;
    my @parts    = @_;

    my $body;
    foreach my $part (@parts) {
        $body .= ref $part eq "ARRAY" ? join " ", @{$part} : $part;
    }
    return unless defined $body;

    ...
}

...

1;
```

----
Note:

  * this will work in included scenario if the include contains two rules: one with `msg_body` and with `msg_part->body`.

----

