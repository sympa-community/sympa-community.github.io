---
title: 'sympa_toc(1)'
---

# NAME

sympa\_toc - Documentation on Sympa - Table of Contents

# DESCRIPTION

**Sympa** is scalable and highly customizable mailing list manager.
It can cope with big lists (200,000 subscribers) and comes with
a complete (user and admin) Web interface. It is
internationalized, and supports the us, fr, de, es, it, fi, and
chinese locales. A scripting language allows you to extend the
behavior of commands. **Sympa** can be linked to an LDAP directory
or an RDBMS to create dynamic mailing lists. **Sympa** provides S/MIME
and HTTPS based authentication and encryption.
Sympa is a modern mailing-list manager. It supports a lot of useful
features.

Below is the list of documentation of Sympa.

## Reference manual

- _Sympa Administration Manual_

    [https://sympa-community.github.io/manual/](https://sympa-community.github.io/manual/).

## Daemons

- [archived(8)](./archived.8.md)

    Mailing List Archiving Daemon for Sympa

- [bounced(8)](./bounced.8.md)

    Mailing List Bounce Processing Daemon for Sympa

- [bulk(8)](./bulk.8.md)

    Daemon for submitting messages to SMTP engine

- [sympa\_automatic(8)](./sympa_automatic.8.md)

    Automatic list creation daemon

- [sympa\_msg(8)](./sympa_msg.8.md)

    Daemon to handle incoming messages

- [task\_manager(8)](./task_manager.8.md)

    Daemon to Process Periodical Sympa Tasks

## Web interface

- [sympa\_soap\_server(8)](./sympa_soap_server.8.md)

    Sympa SOAP server

- [wwsympa(8)](./wwsympa.8.md)

    WWSympa, Sympa's web interface

## Shell Interface

### Administration tools

- [sympa(1)](./sympa.1.md)

    Command line utility to manage Sympa

- [sympa\_newaliases(1)](./sympa_newaliases.1.md)

    Alias database maintenance

- [sympa\_wizard(1)](./sympa_wizard.1.md)

    Help Performing Sympa Initial Setup

### Maintenance tools

- [arc2webarc(1)](./arc2webarc.1.md)

    TBD.

- [init\_comment(1)](./init_comment.1.md)

    TBD.

- [mod2html(1)](./mod2html.1.md)

    Generates HTML view of messages to be moderated

- [p12topem(1)](./p12topem.1.md)

    TBD.

- [sympa\_soap\_client(1)](./sympa_soap_client.1.md)

    TBD.

- [sympa\_test\_ldap(1)](./sympa_test_ldap.1.md)

    Testing LDAP connection for Sympa

- [testlogs(1)](./testlogs.1.md)

    TBD.

- [tpl2tt2(1)](./tpl2tt2.1.md)

    TBD.

- [upgrade\_bulk\_spool(1)](./upgrade_bulk_spool.1.md)

    Migrating messages in bulk tables

- [upgrade\_send\_spool(1)](./upgrade_send_spool.1.md)

    Upgrade messages in incoming spool

- [upgrade\_shared\_repository(1)](./upgrade_shared_repository.1.md)

    Encode file names in shared repositories.

- [upgrade\_sympa\_password(1)](./upgrade_sympa_password.1.md)

    Upgrading password in database

## Auxiliary Programs

- [alias\_manager(8)](./alias_manager.8.md)

    Manage Sympa aliases (Obsoleted)

- [ldap\_alias\_manager(8)](./ldap_alias_manager.8.md)

    TBD.

- [mysql\_alias\_manager(8)](./mysql_alias_manager.8.md)

    TBD.

- [queue(8)](./queue.8.md), [bouncequeue(8)](./bouncequeue.8.md), [familyqueue(8)](./familyqueue.8.md)

    TBD.

- [sympa\_newaliases-wrapper(8)](./sympa_newaliases-wrapper.8.md), [sympa\_soap\_server-wrapper(8)](./sympa_soap_server-wrapper.8.md),
[wwsympa-wrapper(8)](./wwsympa-wrapper.8.md)

    TBD.

## Configuration files

- [auth.conf(5)](./auth.conf.5.md)

    Configuration of authentication mechanisms for web interface of Sympa

- [charset.conf(5)](./charset.conf.5.md)

    Configuration file for legacy character set support by Sympa

- [crawlers\_detection.conf(5)](./crawlers_detection.conf.5.md)

    User agents to be excluded from session management

- [edit\_list.conf(5)](./edit_list.conf.5.md)

    Configuration of privileges to edit list configuration

- [ldap\_alias\_manager.conf(5)](./ldap_alias_manager.conf.5.md)

    Configuration of LDAP alias management

- [list\_config(5)](./list_config.5.md)

    Configuration file for mailing list

- [nrcpt\_by\_domain.conf(5)](./nrcpt_by_domain.conf.5.md)

    Grouping factor for SMTP sessions by recipient domains

- [sympa.conf(5)](./sympa.conf.5.md), robot.conf(5)

    Configuration file for default site and robot

- [sympa\_scenario(5)](./sympa_scenario.5.md)

    Authorization scenario

- [topics.conf(5)](./topics.conf.5.md)

    Configuration of list topics

## Internals

- [Sympa::Internals(3)](./Sympa-Internals.3.md)

    List of Sympa internal modules

- [sympa\_database(5)](./sympa_database.5.md)

    Structure of Sympa core database

# AVAILABILITY

Latest version of **Sympa** is available from
[https://github.com/sympa-community/sympa/releases](https://github.com/sympa-community/sympa/releases).

# MORE DOCUMENTATION

The full documentation in HTML format can be found in
[https://sympa-community.github.io/manual/](https://sympa-community.github.io/manual/).

The mailing lists (with web archives) can be accessed at
[https://listes.renater.fr/sympa/lists/informatique/sympa](https://listes.renater.fr/sympa/lists/informatique/sympa).

# BUGS

Report bugs to Sympa bug tracker.
See [http://www.sympa.org/tracking](http://www.sympa.org/tracking).

# SEE ALSO

[sendmail(8)](./sendmail.8.md).
