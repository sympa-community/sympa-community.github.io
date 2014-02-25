Templates Plugins
=================

----
Warning:

  * This feature is available from Sympa 6.2

----

The template plugins system extends template customization capabilities, it allows you to run your own code to add content or do some processing in templates.

It is then possible to add links to a menu based on privileges from another backend, get data and render it from a webservice and much more.

A plugin consists of a single perl package located in the `bin/Sympa/Template/Plugin` directory under Sympa's root.

The plugin is called in templates with a simple `[% USE myPlugin %]`, it is possible to pass arguments (`[% USE myPlugin("foo") %]`), more examples to follow.

This package must use `Template::Plugin` as base and provide at least 2 subroutines :

| Subroutine | Called when | Basic mode | | Singleton mode | |
|---|---|---|---|---|---|
| | | Arguments | Must return | Arguments | Must return
| load | Plugin is required by perl, that is when first used | Package name and context object | Package name | Package name and context object | Blessed reference to package instance |
| new | Each time the plugin is `USE`d in templates | Package name, context object and given arguments | Blessed reference to package instance | Blessed instance returned by initial `load` call, context object and given arguments |

The blessed instance given in arguments
In the `basic` mode each time the plugin is `USE`d a new instance is created and returned, in the `singleton` mode a single instance is created and return each time the plugin is `USE`d, it can be useful when the plugin connects to a database to use only one socket ...

Plugin skeleton
---------------

``` perl
#!/usr/bin/perl
 
package Sympa::Template::Plugin::myPlugin;
 
use base qw( Template::Plugin );
use Template::Plugin;
 
# Called as Sympa::Template::Plugin::Dokuwiki->load($context)
# by Template engine
sub load {
    my ($class, $context) = @_;
    return $class;
}
 
# Called as Sympa::Template::Plugin::Dokuwiki->new($context, @params)
# when using [% USE Dokuwiki(param1, param2, ...) %] in template
sub new {
    my ($class, $context, @params) = @_;
 
    my $self = {
        'context' => $context # Keep the USE context (template variables ...)
    };
 
    bless $self, $class;
 
    return $self;
}
 
# Package must return trueish value
1;
```

So, how can I use it ?
----------------------

Once you have `USE`d your plugin in a template you can access its properties and call its methods :

``` perl
#!/usr/bin/perl
 
package Sympa::Template::Plugin::myPlugin;
 
use base qw( Template::Plugin );
use Template::Plugin;
 
# Called as Sympa::Template::Plugin::Dokuwiki->load($context)
# by Template engine
sub load {
    my ($class, $context) = @_;
    return $class;
}
 
# Called as Sympa::Template::Plugin::Dokuwiki->new($context, @params)
# when using [% USE Dokuwiki(param1, param2, ...) %] in template
sub new {
    my ($class, $context, @params) = @_;
 
    my $self = {
        'context' => $context, # Keep the USE context (template variables ...)
        'foo' => 'Bar !',
    };
 
    bless $self, $class;
 
    return $self;
}
 
sub bar {
    my ($self, $value) = @_;
 
    return "You gave me ".$value;
}
 
# Package must return trueish value
1;
```

``` code
[% USE myPlugin %] or [% USE p = myPlugin %]

Foo : [% myPlugin.foo %] or [% p.foo %]

Bar : [% myPlugin.bar("foobar") %] or [% p.bar("foobar") %]
```

### Error handling

You can use the plugin error reporting system along with the template engine's `TRY`/`CATCH` mechanism to handle errors :

``` perl
#!/usr/bin/perl
 
package Sympa::Template::Plugin::myPlugin;
 
use base qw( Template::Plugin );
use Template::Plugin;
 
# Called as Sympa::Template::Plugin::Dokuwiki->load($context)
# by Template engine
sub load {
    my ($class, $context) = @_;
    return $class;
}
 
# Called as Sympa::Template::Plugin::Dokuwiki->new($context, @params)
# when using [% USE Dokuwiki(param1, param2, ...) %] in template
sub new {
    my ($class, $context, @params) = @_;
 
    return $class->error('foo is not foo') unless $params[0] eq 'foo';
 
    my $self = {
        'context' => $context, # Keep the USE context (template variables ...)
        'foo' => $param[0],
    };
 
    bless $self, $class;
 
    return $self;
}
 
sub bar {
    my ($self, $value) = @_;
 
    return $self->error('value must be scalar !') if(ref $value);
 
    return "You gave me ".$value;
}
 
# Package must return trueish value
1;
```

``` code
[% TRY %]
    [% USE myPlugin("bar") %]
    [% myPlugin.foo %]
[% CATCH %]
    [% error.info %] # error.info value is the string given to the error method call
[% END %]
```

### Accessing template variables

You can access the templates variables which are in scope by accessing the [context stash](http://www.template-toolkit.org/docs/modules/Template/Stash.html) in your plugin's methods :

``` perl
my $robot = $self->{'context'}->stash()->get('robot'); # Get same value as using [% robot %] in template
 
my $user_email = $self->{'context'}->stash()->get(['user', 0, 'email', 0]); # Get same value as using [% user.email %] in template, 0's after each token are mandatory
```

### Getting the request URL

The request URL is availabe as the `path_info` template variable, retrieving it to handle custom parameters is as simple as :

``` perl
my $path_info = $self->{'context'}->stash()->get('path_info'); # URL https://mylistserver.tld/sympa/foo/bar will give /foo/bar
```

### Using forms

As Sympa filters carefully input parameters a plugin dedicated mechanism has been designed to allow plugins to handle their parameters the way the want/need it.

Your plugin's inputs in your plugin's forms must have names like `plugin.myPlugin.foo`, their values will be set as entries in the `plugin.myPlugin` template variable.

``` perl
#!/usr/bin/perl
 
package Sympa::Template::Plugin::myPlugin;
 
use base qw( Template::Plugin );
use Template::Plugin;
 
# Called as Sympa::Template::Plugin::Dokuwiki->load($context)
# by Template engine
sub load {
    my ($class, $context) = @_;
    return $class;
}
 
# Called as Sympa::Template::Plugin::Dokuwiki->new($context, @params)
# when using [% USE Dokuwiki(param1, param2, ...) %] in template
sub new {
    my ($class, $context, @params) = @_;
 
    my $self = {
        'context' => $context, # Keep the USE context (template variables ...)
    };
 
    bless $self, $class;
 
    return $self;
}
 
sub processForm {
    my ($self) = @_;
 
    my $data = $self->{'context'}->stash()->get(['plugin', 0, 'myPlugin', 0]); # Retreive form data
 
    return 0 unless defined $data->{'save'}; # Was the form submitted
 
    return $self->error('Bad foo !') unless $data->{'foo'} =~/^[0-9]+$/;
 
    # Save foo somewhere
 
    return 1;
}
 
# Package must return trueish value
1;
```

``` html4strict
[% USE myPlugin %]
[% TRY %]
    [% IF myPlugin.processForm() %]
        Ok !
    [% ELSE %]
        <form action="" method="post">
            <input type="text" name="plugin.myPlugin.foo" value="" />
            <input type="submit" name="plugin.myPlugin.save" value="Save" />
        </form>
    [% END %]
[% CATCH %]
    Error while processing form : [% error.info %]
[% END %]
```

### A configuration file for your plugin

Managing configuration directly in your package file can quickly become a hassle, you can rely on Sympa's native configuration seeking and parsing capabilities to help you having nice configuration files, per virtualhost configuration, why not per list configuration with default virtualhost values ...

#### Parsing Sympa style configuration file

``` perl
my $config_structure = {
    'foo' => {
        'format' => '[0-9]+', # parameter validation regexp
        'occurrence' => '1',  # parameter occurence rule
        'case' => 'insensitive',
    },
    'bar' => {
        'format' => '.+',
        'occurence' => '1-n',
        'case' => 'insensitive',
    },
};
 
my $config = Conf::load_generic_conf_file($config_file, $config_structure, 'abort'); # $config will be a reference to a hash of configuration parameters, undef if parsing failed
return $class->error('Config error') unless($config);
```

Configuration file example :

``` code
foo 3
bar abc
bar def5
```

The `format` entry of root parameters can be composite to define a paragraph :

``` perl
my $config_structure = {
    'foo' => {
        'occurrence' => '0-n',
        'format' => {
            'name' => {
                'format' => '[a-z0-9]+',
                'occurrence' => '1',
                'case' => 'insensitive',
            },
            'bar' => {
                'format' => '[0-9]+',
                'occurence' => '1',
                'case' => 'insensitive',
            },
        }
    }
};
```

Then the configuration file will look like :

``` code
foo
    name foo1
    bar 27

foo
    name foo2
    bar 42
```

#### A configuration file aside of the plugin package file

You can put your configuration file in the same directory as your package file, that is the `bin/Sympa/Template/Plugin` directory :

``` perl
my $file = $class;
$file =~ s/::/\//g;
my $config_file = Sympa::Constants::MODULEDIR.'/'.$file.'.conf';
```

or even simpler

``` perl
my $config_file = Sympa::Constants::MODULEDIR.'/Sympa/Template/Plugin/myPlugin.conf';
```

#### A virtualhost specific config file with a default in etc/ dir

Sympa's configuration seeking mechanism work as follows :

-   Look in the virtualhost directory (`etc/<virtualhost>/`) if previous not found

-   Look in the `etc/` directory if previous not found

-   Look in the `default/` if previous not found

You can make use of this mechanism in your plugin to have a global configuration which is overridden for some virtualhosts for example :

``` perl
my $config_file = tools::get_filename('etc', {}, 'myPlugin.conf', $self->{'context'}->stash()->get('robot'));
return $self->error('Config file not found') unless($config_file);
```
