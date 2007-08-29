Web archive
===========

Sympa maintains web archive for mailing lists that have the feature enabled. The web archives are fully integrated in the Sympa web interface, though the engine that does the HTML tranformation is an third-party tool, [MHonArc](http://www.mhonarc.org "http://www.mhonarc.org").

Features
--------

### Access Control

This is probably the most interesting part of the Sympa web archives : you can restrict access to a list web archives to a certain population (members of the current list, members of another list, intranet users, users defined in an SQL/LDAP base). This fine tuning is defined via the [web\_archive.access](/manual/parameters-archives#web_archive "manual:parameters-archives") list parameter. This parameter refers to the corresponding [authorization scenario](/manual/authorization-scenarios#authorization_scenarios "manual:authorization-scenarios").

### Search engine

Sympa provides a search engine to search archive of each list. User can either use the standard search field or the advanced search page. The search engine is based on [MarcSearch](http://www.mhonarc.org/contrib/marc-search/ "http://www.mhonarc.org/contrib/marc-search/") perl library.

### Messages removal

Sympa provides a `message removal` feature, accessible through a button from individual message web pages. This feature is restricted to either message authors (once authenticated) or list owners. Removal requests are then processed by the `archivd.pl` process ; therefore it may take a few seconds before it is removed.

### Message reminder

From individual message web pages, you can ask that the message is sent back to you, via SMTP.

### Export of web archives

List owners have access to an archive management page from the web interface. From this page, they can download a ZIP of their list web archives. The ZIP is organized by month, each month containing original emails.

MHonArc tool
------------

Sympa does not perform the MIME → HTML transformation itself, neither does it build the messages index and threads. All this is performed by the [MHonArc](http://www.mhonarc.org "http://www.mhonarc.org") program. MhonArc is a software used beside many other mailing list softwares. The may Sympa uses MHonArc concentrates on integration.

You can customize the behavior of `mhonarc` via the `mhonarc-ressources.tt2` resource file. The html view of archives is being delivered by the `wwsympa.fcgi` process so the MhonArc output uses Sympa TT2 template format. When modifying `mhonarc-ressources.tt2` you may change the way MhonArc prepare archives or you may change the way wwsympa.fcgi show them.

See [MHonArc manual](http://www.mhonarc.org/MHonArc/doc/mhonarc.html "http://www.mhonarc.org/MHonArc/doc/mhonarc.html") for more informations on customization.

Archives structure
------------------

The web archives are gathered in a common directory defined by the [arc\_path](/manual/web-interface#arc_path "manual:web-interface") wwsympa.conf parameter. This directory contains a subdirecty for each archived mailing list. The list archive directory is structured a subdirectory for each month. Eath month directory contains both the HTML pages for messages and indexes and a subdirectory containing the orginal messsages in a test format.

Here is a sample list archive hierarchy :

``` {.code}
/home/sympa/arc/your-list@cru.fr/:
  1997-07
  ...
  2007-05
    arctxt
      1
      2
      ...
    mail1.html
    msg00000.html
    msg00001.html
    msg00002.html
    ...
    thrd1.html
```

Configuration paramaters
------------------------

#### sympa.conf parameters

`arc_path`, `archive_default_index`, `archived_pidfile`, `mhonarc`.

See [WWSympa, Sympa's web interface](/manual/web-interface "manual:web-interface")

#### wwsympa.conf prameters

`web_archive_spam_protection`, `default_archive_quota`.

See [index](/conf-parameters/index "conf-parameters:index")

#### list parameters

`web_archive`, `archive_crypted_msg`.

See [× Archive related](/manual/parameters-archives "manual:parameters-archives")

Archived.pl daemon
------------------

A dedicated perl script, `archived.pl`, is performing all operations on list web archives (adding a message to archive, removing a message, rebuilding HTML files). This process is running as the `sympa` user ; messages to archive and removal/rebuild commands are fetched from the `outgoing` spool defined by the [queueoutgoing](/conf-parameters/part2#queueoutgoing "conf-parameters:part2") sympa.conf parameter.

Rebuilding web archive
----------------------

Sympa provides a way to rebuild all HTML files for a given web archive. This is useful whenever you changed the `mhonarc-resources.tt2` file or if Sympa changed its template format or the charset used for web pages.

The rebuild feature is accessible to listmasters only from the `Sympa admin` → `Archives` chapter, from the web interface. The rebuild is then run by the `archived.pl` process.

Importing archives
------------------

Since version 5.2b, Sympa maintains a single archive of mailing lists, but it previously maintained both a mail archive (stored in the `expl/mylist/archives/` directories) and a web archive (for historical reasons). You may need to import existing mail archives in a list mail archive, using the provided `arc2webarc.pl` script.

If you are moving from another mailing list software to Sympa, you are also facing messages archive import problems. Check the [Contrib section](http://www.sympa.org/wiki/contribs/index "http://www.sympa.org/wiki/contribs/index") for useful migration tools.
