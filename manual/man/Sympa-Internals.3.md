---
title: 'Sympa::Internals(3)'
---

# NAME

Sympa::Internals - Sympa internals

# DESCRIPTION

Below is the list of Sympa internal modules.
To know details of each module, run:

    man Sympa::ModuleName

## Modules

- [Sympa](./Sympa.3.md)

    Future base class of Sympa functional objects

- [Sympa::Alarm](./Sympa-Alarm.3.md)

    Spool on memory for listmaster notification

- [Sympa::Aliases](./Sympa-Aliases.3.md)

    Base class for alias management

- [Sympa::Aliases::CheckSMTP](./Sympa-Aliases-CheckSMTP.3.md)

    Alias management: Check addresses using SMTP

- [Sympa::Aliases::External](./Sympa-Aliases-External.3.md)

    Alias management: Updating aliases by external program

- [Sympa::Aliases::Template](./Sympa-Aliases-Template.3.md)

    Alias management: Aliases file based on template

- [Sympa::Archive](./Sympa-Archive.3.md)

    Archives of Sympa

- [Sympa::Bulk](./Sympa-Bulk.3.md)

    Spool for bulk sending

- [Sympa::CommandDef](./Sympa-CommandDef.3.md)

    Definition of mail commands

- [Sympa::ConfDef](./Sympa-ConfDef.3.md)

    Definition of site and robot configuration parameters

- [Sympa::Config](./Sympa-Config.3.md)

    List configuration

- [Sympa::Config\_XML](./Sympa-Config_XML.3.md)

    TBD

- [Sympa::Constants](./Sympa-Constants.3.md)

    Definition of constants

- [Sympa::Crash](./Sympa-Crash.3.md)

    Show traceback on critical error

- [Sympa::Database](./Sympa-Database.3.md)

    Handling databases

- [Sympa::DatabaseDescription](./Sympa-DatabaseDescription.3.md)

    Definition of core database structure

- [Sympa::DatabaseDriver](./Sympa-DatabaseDriver.3.md)

    Base class of database drivers for Sympa

- [Sympa::DatabaseDriver::CSV](./Sympa-DatabaseDriver-CSV.3.md)

    Database driver for CSV

- [Sympa::DatabaseDriver::LDAP](./Sympa-DatabaseDriver-LDAP.3.md)

    Database driver for LDAP search operation

- [Sympa::DatabaseDriver::MySQL](./Sympa-DatabaseDriver-MySQL.3.md)

    Database driver for MySQL / MariaDB

- [Sympa::DatabaseDriver::ODBC](./Sympa-DatabaseDriver-ODBC.3.md)

    Database driver for ODBC

- [Sympa::DatabaseDriver::Oracle](./Sympa-DatabaseDriver-Oracle.3.md)

    Database driver for Oracle Database

- [Sympa::DatabaseDriver::Oracle::St](./Sympa-DatabaseDriver-Oracle-St.3.md)

    Correcting behavior of DBD::Oracle

- [Sympa::DatabaseDriver::PostgreSQL](./Sympa-DatabaseDriver-PostgreSQL.3.md)

    Database driver for PostgreSQL

- [Sympa::DatabaseDriver::SQLite](./Sympa-DatabaseDriver-SQLite.3.md)

    Database driver for SQLite

- [Sympa::DatabaseManager](./Sympa-DatabaseManager.3.md)

    Managing schema of Sympa core database

- [Sympa::Datasource](./Sympa-Datasource.3.md)

    TBD

- [Sympa::Family](./Sympa-Family.3.md)

    TBD

- [Sympa::HTML::FormatText](./Sympa-HTML-FormatText.3.md)

    TBD

- [Sympa::HTMLDecorator](./Sympa-HTMLDecorator.3.md)

    Decorating HTML texts

- [Sympa::HTMLSanitizer](./Sympa-HTMLSanitizer.3.md)

    Sanitize HTML contents

- [Sympa::Language](./Sympa-Language.3.md)

    Handling languages and locales

- [Sympa::List](./Sympa-List.3.md)

    TBD

- [Sympa::List::Config](./Sympa-List-Config.3.md)

    List configuration

- [Sympa::List::Users](./Sympa-List-Users.3.md)

    List users

- [Sympa::ListDef](./Sympa-ListDef.3.md)

    Definition of list configuration parameters

- [Sympa::ListOpt](./Sympa-ListOpt.3.md)

    Definition of list configuration parameter values

- [Sympa::LockedFile](./Sympa-LockedFile.3.md)

    Filehandle with locking

- [Sympa::Log](./Sympa-Log.3.md)

    Logging facility of Sympa

- [Sympa::Mailer](./Sympa-Mailer.3.md)

    Store messages to sendmail

- [Sympa::Message](./Sympa-Message.3.md)

    Mail message embedding for internal use in Sympa

- [Sympa::Message::Plugin](./Sympa-Message-Plugin.3.md)

    process hooks

- [Sympa::Message::Plugin::FixEncoding](./Sympa-Message-Plugin-FixEncoding.3.md)

    Example module for message hook to correct charset and encoding of messages

- [Sympa::Message::Template](./Sympa-Message-Template.3.md)

    Mail message generated from template

- [Sympa::Process](./Sympa-Process.3.md)

    Process of Sympa

- [Sympa::Regexps](./Sympa-Regexps.3.md)

    Definition of regular expressions

- [Sympa::Request](./Sympa-Request.3.md)

    Requests for operation

- [Sympa::Request::Collection](./Sympa-Request-Collection.3.md)

    Collection of requests

- [Sympa::Request::Handler](./Sympa-Request-Handler.3.md)

    Base class of request handler classes

- [Sympa::Request::Message](./Sympa-Request-Message.3.md)

    Command message as spool of requests

- [Sympa::Robot](./Sympa-Robot.3.md)

    TBD

- [Sympa::Scenario](./Sympa-Scenario.3.md)

    Authorization scenarios

- [Sympa::Spindle](./Sympa-Spindle.3.md)

    Base class of subclasses to define Sympa workflows

- [Sympa::Spindle::AuthorizeMessage](./Sympa-Spindle-AuthorizeMessage.3.md)

    Workflow to authorize messages bound for lists

- [Sympa::Spindle::AuthorizeRequest](./Sympa-Spindle-AuthorizeRequest.3.md)

    Workflow to authorize requests in command messages

- [Sympa::Spindle::DispatchRequest](./Sympa-Spindle-DispatchRequest.3.md)

    Workflow to dispatch requests

- [Sympa::Spindle::DistributeMessage](./Sympa-Spindle-DistributeMessage.3.md)

    Workflow to distribute messages to list members

- [Sympa::Spindle::DoCommand](./Sympa-Spindle-DoCommand.3.md)

    Workflow to handle command messages

- [Sympa::Spindle::DoForward](./Sympa-Spindle-DoForward.3.md)

    Workflow to forward messages to administrators

- [Sympa::Spindle::DoMessage](./Sympa-Spindle-DoMessage.3.md)

    Workflow to handle messages bound for lists

- [Sympa::Spindle::ProcessArchive](./Sympa-Spindle-ProcessArchive.3.md)

    Workflow of archive storage

- [Sympa::Spindle::ProcessAuth](./Sympa-Spindle-ProcessAuth.3.md)

    Workflow of request confirmation

- [Sympa::Spindle::ProcessAutomatic](./Sympa-Spindle-ProcessAutomatic.3.md)

    Workflow of automatic list creation

- [Sympa::Spindle::ProcessBounce](./Sympa-Spindle-ProcessBounce.3.md)

    Workflow of bounce processing

- [Sympa::Spindle::ProcessDigest](./Sympa-Spindle-ProcessDigest.3.md)

    Workflow of digest sending

- [Sympa::Spindle::ProcessHeld](./Sympa-Spindle-ProcessHeld.3.md)

    Workflow of message confirmation

- [Sympa::Spindle::ProcessIncoming](./Sympa-Spindle-ProcessIncoming.3.md)

    Workflow of processing incoming messages

- [Sympa::Spindle::ProcessMessage](./Sympa-Spindle-ProcessMessage.3.md)

    Workflow of command processing

- [Sympa::Spindle::ProcessModeration](./Sympa-Spindle-ProcessModeration.3.md)

    Workflow of message moderation

- [Sympa::Spindle::ProcessOutgoing](./Sympa-Spindle-ProcessOutgoing.3.md)

    Workflow of message distribution

- [Sympa::Spindle::ProcessRequest](./Sympa-Spindle-ProcessRequest.3.md)

    Workflow of request processing

- [Sympa::Spindle::ProcessTask](./Sympa-Spindle-ProcessTask.3.md)

    Workflow of task processing

- [Sympa::Spindle::ProcessTemplate](./Sympa-Spindle-ProcessTemplate.3.md)

    Workflow of template sending

- [Sympa::Spindle::ResendArchive](./Sympa-Spindle-ResendArchive.3.md)

    Workflow of resending messages in archive

- [Sympa::Spindle::ToAlarm](./Sympa-Spindle-ToAlarm.3.md)

    Process to store messages into spool on memory for listmaster notification

- [Sympa::Spindle::ToArchive](./Sympa-Spindle-ToArchive.3.md)

    Process to store messages into archiving spool

- [Sympa::Spindle::ToAuth](./Sympa-Spindle-ToAuth.3.md)

    Process to store requests into request spool to wait for moderation

- [Sympa::Spindle::ToAuthOwner](./Sympa-Spindle-ToAuthOwner.3.md)

    Process to store requests into request spool to wait for moderation

- [Sympa::Spindle::ToDigest](./Sympa-Spindle-ToDigest.3.md)

    Process to store messages into digest spool

- [Sympa::Spindle::ToEditor](./Sympa-Spindle-ToEditor.3.md)

    Process to forward messages to list editors

- [Sympa::Spindle::ToHeld](./Sympa-Spindle-ToHeld.3.md)

    Process to store messages into held spool to wait for confirmation

- [Sympa::Spindle::ToList](./Sympa-Spindle-ToList.3.md)

    Process to distribute messages to list members

- [Sympa::Spindle::ToMailer](./Sympa-Spindle-ToMailer.3.md)

    Process to store messages into sendmail component

- [Sympa::Spindle::ToModeration](./Sympa-Spindle-ToModeration.3.md)

    Process to store messages into held spool to wait for moderation

- [Sympa::Spindle::ToOutgoing](./Sympa-Spindle-ToOutgoing.3.md)

    Process to store messages into outgoing spool

- [Sympa::Spindle::TransformDigestFinal](./Sympa-Spindle-TransformDigestFinal.3.md)

    Process to transform digest messages - final stage

- [Sympa::Spindle::TransformIncoming](./Sympa-Spindle-TransformIncoming.3.md)

    Process to transform messages - first stage

- [Sympa::Spindle::TransformOutgoing](./Sympa-Spindle-TransformOutgoing.3.md)

    Process to transform messages - second stage

- [Sympa::Spool](./Sympa-Spool.3.md)

    Base class of spool classes

- [Sympa::Spool::Archive](./Sympa-Spool-Archive.3.md)

    Spool for messages waiting for archiving

- [Sympa::Spool::Auth](./Sympa-Spool-Auth.3.md)

    Spool for held requests waiting for moderation

- [Sympa::Spool::Automatic](./Sympa-Spool-Automatic.3.md)

    Spool for incoming messages in automatic spool

- [Sympa::Spool::Bounce](./Sympa-Spool-Bounce.3.md)

    Spool for incoming bounce messages

- [Sympa::Spool::Digest](./Sympa-Spool-Digest.3.md)

    Spool for messages waiting for digest sending

- [Sympa::Spool::Digest::Collection](./Sympa-Spool-Digest-Collection.3.md)

    Collection of digest spools

- [Sympa::Spool::Held](./Sympa-Spool-Held.3.md)

    Spool for held messages waiting for confirmation

- [Sympa::Spool::Incoming](./Sympa-Spool-Incoming.3.md)

    Spool for incoming messages

- [Sympa::Spool::Moderation](./Sympa-Spool-Moderation.3.md)

    Spool for held messages waiting for moderation

- [Sympa::Spool::Task](./Sympa-Spool-Task.3.md)

    Spool for tasks

- [Sympa::Task](./Sympa-Task.3.md)

    Tasks of Sympa

- [Sympa::Template](./Sympa-Template.3.md)

    Template parser

- [Sympa::Ticket](./Sympa-Ticket.3.md)

    One-time ticket for authorization

- [Sympa::Tools::Data](./Sympa-Tools-Data.3.md)

    TBD

- [Sympa::Tools::DKIM](./Sympa-Tools-DKIM.3.md)

    TBD

- [Sympa::Tools::Domains](./Sympa-Tools-Domains.3.md)

    Domains-related functions

- [Sympa::Tools::File](./Sympa-Tools-File.3.md)

    File-related functions

- [Sympa::Tools::Password](./Sympa-Tools-Password.3.md)

    TBD

- [Sympa::Tools::SMIME](./Sympa-Tools-SMIME.3.md)

    TBD

- [Sympa::Tools::Text](./Sympa-Tools-Text.3.md)

    Text-related functions

- [Sympa::Tools::Time](./Sympa-Tools-Time.3.md)

    Time-related functions

- [Sympa::Topic](./Sympa-Topic.3.md)

    Message topic

- [Sympa::Tracking](./Sympa-Tracking.3.md)

    Spool for message tracking

- [Sympa::Upgrade](./Sympa-Upgrade.3.md)

    TBD

- [Sympa::User](./Sympa-User.3.md)

    All Users Identified by Sympa

- [Sympa::WWW::Auth](./Sympa-WWW-Auth.3.md)

    TBD

- [Sympa::WWW::Marc](./Sympa-WWW-Marc.3.md)

    TBD

- [Sympa::WWW::Marc::Search](./Sympa-WWW-Marc-Search.3.md)

    Search archives of Sympa

- [Sympa::WWW::Report](./Sympa-WWW-Report.3.md)

    TBD

- [Sympa::WWW::Session](./Sympa-WWW-Session.3.md)

    Web session

- [Sympa::WWW::SharedDocument](./Sympa-WWW-SharedDocument.3.md)

    Shared document repository and its nodes

- [Sympa::WWW::SOAP](./Sympa-WWW-SOAP.3.md)

    TBD

- [Sympa::WWW::SOAP::Transport](./Sympa-WWW-SOAP-Transport.3.md)

    TBD

- [Sympa::WWW::Tools](./Sympa-WWW-Tools.3.md)

    TBD

## Workflow

See [Sympa::Internals::Workflow](./Sympa-Internals-Workflow.3.md).

# SEE ALSO

[sympa\_toc(1)](./sympa_toc.1.md).

_Sympa Administration Manual_.
[https://sympa-community.github.io/manual/](https://sympa-community.github.io/manual/).
