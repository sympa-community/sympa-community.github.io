Dokiwiki plugin
===============

This plugin offers a set of functionalities, from simple Sympa-based authorization to full coupling between lists and wiki farm.

Sympa based authorization for DokuWiki
--------------------------------------

The idea is to authorize a user to access part of the wiki if he/she is subscribed to a list.

Using Sympa's SOAP server one can get the user's lists, we just need a way for DokuWiki to automatically do it and add the lists as groups to the user definition.

[This plugin](/_media/contribs/dokuwiki/sympagroups.zip) does just that (you can copy-paste the link in your wiki's plugin manager page).

| Configuration parameters |

| Name | Type | Signification |
|---|---|---|
| enabled | boolean | Connect user groups to Sympa |
| mailSource | "user" or "mail" | User info to use as email address while fetching lists from Sympa |
| exclusive | boolean | Sympa groups only (don't load wiki groups) |
| soapService | string | URL to the Sympa server's SOAP wsdl |
| applicationID | string | Application ID to be used for authentication |
| applicationPwd | string | Password to be used for authentication |

DokuWiki based lists wikis
--------------------------

One can easily add list wikis by installing a [DokuWiki](https://www.dokuwiki.org/dokuwiki) farm along with a Sympa TT2 plugin allowing to interact with the farm API.

The wiki farm doesn't need to be located on the same machine as Sympa, being in the same domain may help for authentication sharing though.

### Installing DokuWiki and the farm plugin

In order to setup the farm you must first install a recent [DokuWiki](https://www.dokuwiki.org/dokuwiki), preferably under the following layout :

``` code
/<farm_root>
  /farmer => DokuWiki engine
  /barn => where all wiki data and configs will be stored
```

And have your webserver alias `/wiki` to `/<farm_root>/farmer` and `AllowOverride All` on this path.

Next you have to install the ~~[farm plugin](/_media/contribs/dokuwiki/sympagroups.zip)~~ (you can copy-paste the link in your wiki's plugin manager page).

You may also want to use ~~[sympagroups](/templates_plugins/sympagroups)~~.

If you use SSO ~~[genericsso](/templates_plugins/genericsso)~~ may also interest you.

### Enabling farming

#### Add tweak files

You then have to install the preload file under `/<farm_root>/farmer/inc` (see [DokuWiki preload mecanism](https://www.dokuwiki.org/devel:preload)) :

[preload.php](/_export/code/templates_plugins/dokuwiki_plugin?codeblock=0)  
``` code
<?php
 
/**
 * DokuWiki farm oriented preloader
 *
 * @license    GPL 2 (http://www.gnu.org/licenses/gpl.html)
 * @author     Etienne MELEARD <etienne.meleard AT renater.fr>
 */
 
// Config section
 
define('DOKU_FARM_ROOT', '/var/www/wikifarm/');
define('DOKU_FARM_BARN', DOKU_FARM_ROOT.'barn/');
define('DOKU_FARM_MULTIHOSTING', true);
 
 
// /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\
// /!\                                                                     /!\
// /!\  Do not alter after this point unless you know what you are doing   /!\
// /!\                                                                     /!\
// /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\ /!\
 
if(!DOKU_FARM_ROOT || !@is_dir(DOKU_FARM_ROOT)) die('You must set the DOKU_FARM_ROOT constant in '.__FILE__.' in order for the wiki farm to work');
if(!DOKU_FARM_BARN || !@is_dir(DOKU_FARM_BARN)) die('You must set the DOKU_FARM_BARN constant in '.__FILE__.' in order for the wiki farm to work');
 
if(!defined('DOKU_FARM_MULTIHOSTING')) define('DOKU_FARM_MULTIHOSTING', false);
 
$host = $_SERVER['SERVER_NAME'];
$bits = array_values(array_filter(explode('/', $_SERVER['REQUEST_URI'])));
$id = (count($bits) > 1) ? $bits[1] : null;
if($id == '_farmer_') $id = null;
 
$animal_id = null;
if($host && $id) $animal_id = (DOKU_FARM_MULTIHOSTING ? $host.'/' : '').$id;
if(!@is_dir(DOKU_FARM_BARN.$animal_id)) $animal_id = null;
 
define('DOKU_FARM_ANIMAL_ID', $animal_id);
define('DOKU_FARM_IS_FARMER', !(bool)$animal_id);
 
define('DOKU_FARM_HOST', preg_replace('`\:[0-9]+$`', '', $_SERVER['SERVER_NAME']));
 
define('DOKU_FARM_BASEPATH', $animal_id ? DOKU_FARM_BARN.$animal_id.'/' : DOKU_INC);
define('DOKU_FARM_BASEURL', ((isset($_SERVER['HTTPS']) && ($_SERVER['HTTPS'] == 'on')) ? 'https' : 'http').'://'.$_SERVER['SERVER_NAME'].'/');
define('DOKU_FARM_BASEDIR', '/wiki/'.($animal_id ? $id : '_farmer_').'/');
 
define('DOKU_DEFAULT', DOKU_INC.'conf/');
define('DOKU_CONF', DOKU_FARM_BASEPATH.'conf/');
 
$plugin_protected = array(DOKU_DEFAULT.'plugins.required.php');
if($animal_id) $plugin_protected[] = DOKU_DEFAULT.'plugins.barn.protected.php';
 
$conf_protected = array(DOKU_DEFAULT.'farm.protected.php');
if($animal_id) $conf_protected[] = DOKU_DEFAULT.'barn.protected.php';
 
$conf_protected[] = DOKU_CONF.'protected.php';
 
global $config_cascade;
$config_cascade = array(
    'main' => array(
        'default'   => array(DOKU_DEFAULT.'dokuwiki.php'),
        'local'     => array(DOKU_CONF.'local.php'),
        'protected' => $conf_protected,
    ),
    'acronyms'  => array(
        'default'   => array(DOKU_DEFAULT.'acronyms.conf'),
        'local'     => array(DOKU_CONF.'acronyms.conf'),
    ),
    'entities'  => array(
        'default'   => array(DOKU_DEFAULT.'entities.conf'),
        'local'     => array(DOKU_CONF.'entities.conf'),
    ),
    'interwiki' => array(
        'default'   => array(DOKU_DEFAULT.'interwiki.conf'),
        'local'     => array(DOKU_CONF.'interwiki.conf'),
    ),
    'license' => array(
        'default'   => array(DOKU_DEFAULT.'license.php'),
        'local'     => array(DOKU_CONF.'license.php'),
    ),
    'mediameta' => array(
        'default'   => array(DOKU_DEFAULT.'mediameta.php'),
        'local'     => array(DOKU_CONF.'mediameta.php'),
    ),
    'mime'      => array(
        'default'   => array(DOKU_DEFAULT.'mime.conf'),
        'local'     => array(DOKU_CONF.'mime.conf'),
    ),
    'scheme'    => array(
        'default'   => array(DOKU_DEFAULT.'scheme.conf'),
        'local'     => array(DOKU_CONF.'scheme.conf'),
    ),
    'smileys'   => array(
        'default'   => array(DOKU_DEFAULT.'smileys.conf'),
        'local'     => array(DOKU_CONF.'smileys.conf'),
    ),
    'wordblock' => array(
        'default'   => array(DOKU_DEFAULT.'wordblock.conf'),
        'local'     => array(DOKU_CONF.'wordblock.conf'),
    ),
    'acl'       => array(
        'default'   => DOKU_CONF.'acl.auth.php',
    ),
    'plainauth.users' => array(
        'default'   => DOKU_CONF.'users.auth.php',
    ),
    'plugins' => array(
        'local'     => array(DOKU_CONF.'plugins.local.php'),
        'protected' => $plugin_protected,
    ),
    'userstyle' => array(
        'default' => DOKU_CONF.'userstyle.css',
        'print'   => DOKU_CONF.'printstyle.css',
        'feed'    => DOKU_CONF.'feedstyle.css',
        'all'     => DOKU_CONF.'allstyle.css',
    ),
    'userscript' => array(
        'default' => DOKU_CONF.'userscript.js'
    ),
);
```

You will most likely have to tweak the first 3 `define` at the top of the file.

Then you have to install the farm dedicated rewrite file under `/<farm_root>/farmer/.htaccess` :

[.htaccess](/_export/code/templates_plugins/dokuwiki_plugin?codeblock=1)  
``` code
## Enable this to restrict editing to logged in users only
 
## You should disable Indexes and MultiViews either here or in the
## global config. Symlinks maybe needed for URL rewriting.
Options -Indexes -MultiViews +FollowSymLinks
 
## make sure nobody gets the htaccess, README, COPYING or VERSION files
<Files ~ "^([\._]ht|README$|VERSION$|COPYING$)">
    Order allow,deny
    Deny from all
    Satisfy All
</Files>
 
## Uncomment these rules if you want to have nice URLs using
## $conf['userewrite'] = 1 - not needed for rewrite mode 2
RewriteEngine on
 
## Not all installations will require the following line.  If you do, 
## change "/dokuwiki" to the path to your dokuwiki directory relative
## to your document root.
#RewriteBase /wiki
 
## If you enable DokuWikis XML-RPC interface, you should consider to
## restrict access to it over HTTPS only! Uncomment the following two
## rules if your server setup allows HTTPS.
#RewriteCond %{HTTPS} !=on
#RewriteRule ^lib/exe/xmlrpc.php$      https://%{SERVER_NAME}%{REQUEST_URI} [L,R=301]
 
#RewriteRule ^_media/(.*)              lib/exe/fetch.php?media=$1  [QSA,L]
#RewriteRule ^_detail/(.*)             lib/exe/detail.php?media=$1  [QSA,L]
#RewriteRule ^_export/([^/]+)/(.*)     doku.php?do=export_$1&id=$2  [QSA,L]
#RewriteRule ^$                        doku.php  [L]
#RewriteCond %{REQUEST_FILENAME}       !-f
#RewriteCond %{REQUEST_FILENAME}       !-d
#RewriteRule (.*)                      doku.php?id=$1  [QSA,L]
#RewriteRule ^index.php$               doku.php
 
 
# Custom rules for sympa wiki farm
# --------------------------------
 
RewriteBase /
 
# Go to portal if no animal specified
RewriteRule ^$             https://%{HTTP_HOST} [L,DPI,R=301]
 
# Rewrite accesses to animal root to DW main entry point
RewriteCond %{REQUEST_FILENAME}         !-f
RewriteCond %{REQUEST_FILENAME}        !-d
RewriteRule ^([^/]+)/?$                        /wiki/doku.php [QSA,L,DPI]
 
# Rewrite accesses to animal sub-section to the sub-path (strip animal name)
RewriteCond %{REQUEST_FILENAME}            !-f
RewriteCond %{REQUEST_FILENAME}            !-d
RewriteRule ^([^/]+)/(.*)                  /$2 [QSA,DPI]
 
# Here the animal name have been stripped, we don't need to care about it anymore ...
 
# Redirect accesses to the xmlrpc endpoint to https
#RewriteCond %{HTTPS} !=on
#RewriteRule ^/?lib/exe/xmlrpc.php$   https://%{SERVER_NAME}%{REQUEST_URI} [L,R=301]
 
# DW shorthands
RewriteRule ^/?_media/(.*)         /wiki/lib/exe/fetch.php?media=$1  [QSA,L,DPI]
RewriteRule ^/?_detail/(.*)                                /wiki/lib/exe/detail.php?media=$1  [QSA,L,DPI]
RewriteRule ^/?_export/([^/]+)/(.*)                            /wiki/doku.php?do=export_$1&id=$2  [QSA,L,DPI]
 
# Home
RewriteRule ^/?$                   /wiki/doku.php  [L,DPI]
 
# DW lib endpoints should not be considered as sub-sections
RewriteCond %{REQUEST_FILENAME}            !-f
RewriteCond %{REQUEST_FILENAME}            !-d
RewriteRule ^/?lib/(.*)                            /wiki/lib/$1  [QSA,L,DPI]
 
# Sub-section
RewriteCond %{REQUEST_FILENAME}        !-f
RewriteCond %{REQUEST_FILENAME}            !-d
RewriteRule /?(.*)                             /wiki/doku.php?id=$1  [QSA,L,DPI]
 
# Home access through index.php endpoint
RewriteRule ^/?index.php$          /wiki/doku.php [QSA,L,DPI]
```

From this point your *farmer* will be accessible under `/wiki/`*farmer* instead of `/wiki`.

#### Configuration

You may now configure your farm using the following files (merged in the specified order) :

  - Configuration parameters

      - `/<farm_root>/farmer/conf/farm.protected.php` : config shared by the whole farm (farmer and animals), cannot be edited from the web

      - `/<farm_root>/farmer/conf/barn.protected.php` : config shared by all animals (but not farmer), cannot be edited from the web

      - `/<farm_root>/farmer/conf/protected.php` : farmer config, cannot be edited from the web

      - `/<farm_root>/farmer/conf/local.php` : farmer config

  - Plugins

      - `/<farm_root>/farmer/conf/plugins.required.php` : plugins enable/disable for the whole farm, cannot be edited from the web

      - `/<farm_root>/farmer/conf/plugins.barn.required.php` : plugins enable/disable for animals (but not farmer), cannot be edited from the web

Some parameters you have to set :

[farm.protected.php](/_export/code/templates_plugins/dokuwiki_plugin?codeblock=2)  
``` code
<?php
 
$conf['savedir']     = DOKU_FARM_BASEPATH.'data';          // where to store all the files, do not change
$conf['basedir']     = DOKU_FARM_BASEDIR;                // absolute dir from serveroot, do not change
$conf['baseurl']     = DOKU_FARM_BASEURL;                // URL to server including protocol, do not change
$conf['useacl']      = 1;                // Use Access Control Lists to restrict access
$conf['userewrite']  = 1;                //this makes nice URLs with .htaccess
 
// To use genericsso authentication with Shibboleth
//$conf['authtype']    = 'genericsso';
//$conf['disableactions'] = 'register,resendpwd,profile';
//$conf['plugin']['genericsso']['emailAttribute'] = 'mail';
//$conf['plugin']['genericsso']['alwaysCheck'] = 1;
//$conf['plugin']['genericsso']['loginURL'] = 'http://'.$_SERVER['SERVER_NAME'].'/Shibboleth.sso/Login?target={target}';
//$conf['plugin']['genericsso']['logoutURL'] = 'http://'.$_SERVER['SERVER_NAME'].'/Shibboleth.sso/Logout?return={target}';
 
// To use sympagroups
//$conf['plugin']['sympagroups']['enabled'] = 1;
//$conf['plugin']['sympagroups']['mailSource'] = 'mail';
//$conf['plugin']['sympagroups']['exclusive'] = 1;
//$conf['plugin']['sympagroups']['soapService'] = 'https://'.$_SERVER['SERVER_NAME'].'/sympa/wsdl';
//$conf['plugin']['sympagroups']['applicationId'] = 'wiki';
//$conf['plugin']['sympagroups']['applicationPwd'] = 'foobar';
</code>
 
<file php barn.protected.php>
<?php
 
$conf['updatecheck'] = 0;                  // no auto update check for animals
$conf['disableactions'] .= ',check';       // no check command for animals
$conf['remote']      = 0;                  // no API (recommended unless another plugin needs it)
```

<!-- -->

[plugins.barn.protected.php](/_export/code/templates_plugins/dokuwiki_plugin?codeblock=3)  
``` code
<?php
 
$plugins['plugin']      = 0;   // no plugin management for animals
$plugins['testing']      = 0;  // no testing either
 
// If using genericsso
//$plugins['usermanager'] = 0;
```

<!-- -->

[protected.php](/_export/code/templates_plugins/dokuwiki_plugin?codeblock=4)  
``` code
<?php
 
$conf['remote']      = 1;   // enable API for the farmer
```

#### Allow API access from Sympa

To enable access from the remote Sympa server to the farm API you must define the `/<farm_root>/farmer/conf/farm_remote_applications.conf` :

[farm_remote_applications.conf](/_export/code/templates_plugins/dokuwiki_plugin?codeblock=5)  
``` code
# Farm remote management applications secrets and authorized methods
remote_application sympa
secret some_secret_of_your_choice
methods *
```

### Tweak Sympa

#### TT2 plugin

You will need the following CPAN modules : `RPC::XML`, `JSON::XS`

Create the directory `/<sympa_bin_path>/Sympa/Template/Plugin` and create the folowing files under it :

[Dokuwiki.pm](/_export/code/templates_plugins/dokuwiki_plugin?codeblock=6)  
``` code
#!/usr/bin/perl
 
package Sympa::Template::Plugin::Dokuwiki;
 
use base qw( Template::Plugin );
use Template::Plugin;
 
use RPC::XML;
use RPC::XML::Client;
use JSON;
use Digest::SHA;
 
# Called as Sympa::Template::Plugin::Dokuwiki->load($context)
# by TT2 engine
sub load {
    my ($class, $context) = @_;
    return $class;
}
 
# Called as Sympa::Template::Plugin::Dokuwiki->new($context, @params)
# when using [% USE Dokuwiki(param1, param2, ...) %] in TT2 template
sub new {
    my ($class, $context, @params) = @_;
 
    my $stash = $context->stash();
 
    my $self = {
        'context' => $context,
        'robot' => $stash->get('robot'),
        'base_url' => $stash->get('base_url'),
        'sympa_url' => $stash->get(['conf', 0, 'wwsympa_url', 0]),
        'in' => $stash->get(['plugin', 0, 'dokuwiki', 0]), # Get plugin.dokuwiki.* query vars
        'config' => {},
        'wikiname' => undef,
        'xmlrpc_client' => undef,
    };
 
    $self->{'in'} = {} unless defined $self->{'in'};
 
    my $config_structure = {
        'remote_dokuwiki' => {
            'occurrence' => '0-n',
            'format' => {
                'name' => {
                    'format' => '.+',
                    'occurrence' => '1',
                    'case' => 'insensitive',
                },
                'xmlrpc_service_url' => {
                    'format' => '.+',
                    'occurence' => '1',
                    'case' => 'insensitive',
                },
                'application_name' => {
                    'format' => '.+',
                    'occurrence' => '1',
                    'case' => 'insensitive',
                },
                'secret' => {
                    'format' => '.+',
                    'occurrence' => '1'
                }
            }
        }
    };
 
    my $file = $class;
    $file =~ s/::/\//g;
    my $config_file = Sympa::Constants::MODULEDIR.'/'.$file.'.conf';
 
    my $config = Conf::load_generic_conf_file($config_file, $config_structure, 'abort');
    return $class->error('Config error') unless($config);
 
    foreach my $key (keys %$config) {
        if($key eq 'remote_dokuwiki') {
            $self->{'config'}{'remote_dokuwiki'} = {};
            foreach my $remote (@{$config->{$key}}) {
                $self->{'config'}{'remote_dokuwiki'}{$remote->{'name'}} = $remote;
            }
        }else{
            $self->{'config'}{$key} = $config->{$key};
        }
    }
 
    bless $self, $class;
 
    return $class->error('No remote dokuwiki selected') if($#params == -1);
    return $class->error('Unknown remote dokuwiki') unless(defined $self->{'config'}{'remote_dokuwiki'}{$params[0]});
    $self->{'config'}{'remote_dokuwiki'} = $self->{'config'}{'remote_dokuwiki'}{$params[0]};
 
    foreach my $k (keys %{$self->{'config'}{'remote_dokuwiki'}}) {
        my $base_url = $self->{'base_url'};
        $self->{'config'}{'remote_dokuwiki'}{$k} =~ s/\(%\s*base_url\s*%\)/$base_url/g;
        my $robot = $self->{'robot'};
        $self->{'config'}{'remote_dokuwiki'}{$k} =~ s/\(%\s*robot\s*%\)/$robot/g;
    }
 
    $self->{'xmlrpc_client'} = RPC::XML::Client->new($self->{'config'}{'remote_dokuwiki'}{'xmlrpc_service_url'});
 
    if($#params > 0) {
        return $class->error('Wiki does not exist') unless $self->exists($params[1]);
        $self->{'wikiname'} = $params[1];
    }
 
    return $self;
}
 
# Hande wiki creation / deletion through form
sub handleform {
    my ($self, $wikiname, $list) = @_;
 
    my $create = defined $self->{'in'}{'create_wiki'};
    my $delete = defined $self->{'in'}{'delete_wiki'};
    my $template = $self->{'in'}{'template'};
    my $exists = $self->exists($wikiname, 1);
 
    return $self->throw('You can\'t create and delete a wiki at the same time') if($create and $delete);
 
    return $self->throw('The wiki already exists') if($create and $exists);
 
    return $self->throw('The wiki doesn\'t exists') if($delete and not $exists);
 
    if($create) {
        my $customizations = {
            'placeholders' => {
                'list' => $list,
                'robot' => $self->{'robot'}, 
                'sympa_url' => $self->{'sympa_url'}
            }
        };
 
        unless($self->create($wikiname, $template, $customizations)) {
            return $self->throw('Wiki creation failed');
        }
 
        return 'created';
    }
 
    if($delete) {
        unless($self->delete($wikiname)) {
            return $self->throw('Wiki deletion failed');
        }
 
        return 'deleted';
    }
}
 
# Check if a wiki exists
sub exists {
    my ($self, $wikiname, $ignoreinput) = @_;
 
    # We may be creating / deleting this wiki
    if(defined $self->{'in'}{'wikiname'} and $self->{'in'}{'wikiname'} eq $wikiname and not $ignoreinput) {
        return 1 if defined $self->{'in'}{'create_wiki'};
        return 0 if defined $self->{'in'}{'delete_wiki'};
    }
 
    return $self->_call('exists', $wikiname);
}
 
# Create a wiki
sub create {
    my ($self, $wikiname, $template, $customisations) = @_;
 
    return $self->_call('create', $wikiname, $template, $customisations);
}
 
# Deletes a wiki
sub delete {
    my ($self, $wikiname) = @_;
 
    return $self->_call('delete', $wikiname);
}
 
# Make a signed call to the XMLRPC service
sub _call {
    my $self = shift;
    my $method = shift;
    my @args = @_;
 
    my @data = ();
    foreach my $a (@args) {
        if(ref $a eq 'HASH' or ref $a eq 'ARRAY') {
            push @data, JSON->new->canonical()->encode($a);
        }else{
            push @data, $a;
        }
    }
 
    my $signature = Digest::SHA::hmac_sha1_hex(join('&', @data), $self->{'config'}{'remote_dokuwiki'}{'secret'});
    unshift @args, $signature;
    unshift @args, $self->{'config'}{'remote_dokuwiki'}{'application_name'};
 
    print STDERR 'Calling XMLRPC method '.$method.'('.join(', ', @args).')'."\n";
 
    $result = $self->{'xmlrpc_client'}->send_request('plugin.farm.'.$method, @args);
 
    unless(ref $result) {
        print STDERR 'RPC::XML::Client '.$result."\n";
        return $self->throw('Wiki farm request failed');
    }
 
    if($result->is_fault) {
        print STDERR 'RPC::XML::fault '.$result->string.' ('.$result->code.')'."\n";
        return $self->throw('Wiki farm request failed');
    }
 
    return $result->value;
}
 
sub throw {
    my ($self, $error) = @_;
    die (Template::Exception->new('File', $error));
}
 
# Package must return trueish value
1;
```

<!-- -->

[Dokuwiki.conf](/_export/code/templates_plugins/dokuwiki_plugin?codeblock=7)  
``` code
remote_dokuwiki
    name groupware_wikis
    xmlrpc_service_url (%base_url%)/wiki/_farmer_/lib/exe/xmlrpc.php
    application_name sympa
    secret the_secret_you_choose_previously
```

#### Sympa interface tweaking

##### Add create/delete wiki section in list admin

Add the folowing snippet to your `admin.tt2` template :

``` html4strict
[% IF is_privileged_owner %]
    [% TRY %]
        [% USE Dokuwiki("groupware_wikis") %]
<div>
    <form name="manage_list_wiki" action="[% path_cgi %]" method="post">
        <input type="hidden" name="action" value="admin" />
        <input type="hidden" name="list" value="[% list %]" />
        <input type="hidden" name="plugin.dokuwiki.wikiname" value="[% robot %]/[% list %]" />
        <input type="hidden" name="plugin.dokuwiki.template" value="[% robot %]/_template_" />
        <fieldset>
            [% TRY %]
                [% done = Dokuwiki.handleform("${robot}/${list}", "${list}") %]
                [% IF done %]
                    [% IF done == 'created' %]
            <p class="success">The wiki has been created.</p>
                    [% END %]
 
                    [% IF done == 'deleted' %]
            <p class="success">The wiki has been deleted.</p>
                    [% END %]
                [% END %]
                [% IF Dokuwiki.exists("${robot}/${list}") %]
            <input class="MainMenuLinks" type="submit" name="plugin.dokuwiki.delete_wiki" value="[%|loc%]Delete wiki[%END%]" onClick="return request_confirm('[% FILTER escape_quote %][%|loc(list)%]Are you sure you wish to delete this list's wiki ?[%END%][%END%]');"/> [%|loc%]Warning : deletion is final, data recovery is not possible once done.[%END%]
                [% ELSE %]
            <input class="MainMenuLinks" type="submit" name="plugin.dokuwiki.create_wiki" value="[%|loc%]Create wiki[%END%]" /> [%|loc%]Please check out <a href="#">the wiki service terms and conditions</a>[%END%]
                [% END %]
            [% CATCH %]
            <p class="error">[% error.info %]</p>
            [% END %]
        </fieldset>
    </form>
</div>
<br/>
    [% CATCH %]
    <p class="error">[% error.info %]</p>
    [% END %]
[% END %]
```

##### Add a link to the list wiki in the list menu

Add the folowing snippet to your `additional_list_menu_links.tt2` template :

``` html4strict
[% TRY %]
[% USE Dokuwiki("groupware_wikis", "${robot}/${list}") %]
<li><a href="/wiki/[%list%]/">Wiki</a></li>
[% CATCH %]
[% END %]
```

Using federated auth in the lists wikis
---------------------------------------

If you are using SSO login (like shibboleth) for Sympa you can use ~~[this plugin](/_media/contribs/dokuwiki/genericsso.zip)~~ to add the same login to your wiki (genericsso is a plugin that fetch an environment variable and use it to create a Dokuwiki session).

| Configuration parameters |

| Name | Type | Signification |
|---|---|---|
| emailAttribute | string | Environment variable to use as the user id and email |
| alwaysCheck | boolean | Does the plugin needs to check if the SP session is still open for each request (otherwise the wiki session will only be closed at logout) |
| loginURL | string | URL to use for loging-in, may contain `{target}` placeholder that will be replaced by the URL to return to after login |
| logoutURL | string | URL to use for loging-out, may contain `{target}` placeholder that will be replaced by the URL to return to after logout |
