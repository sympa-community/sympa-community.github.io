---
title: 'Bounce management'
up: ../customize.md#customizing-sympa-services
---

Bounce management
=================

Automatic bounce management is a key feature in a modern mailing list server. Sympa provide automatic handling of delivery Status notification (DSN) and message disposition notification (MDN). This is mainly used in order to detect wrong subscriber email address and remove them from list. But you can use it to block list with too many wrong addresses or for message tracking.

The default Sympa configuration is good enough to automatically remove users that are in error for a long time and keep bounce rate low enough. This is important because list server with many invalid address may be blacklisted as a spam source. In addition, sending messages to wrong address is usefulness and may stress the relaying MTA.

About message tracking feature, see also "[Message tracking](../customize/message-tracking.md)".

Setup
-----

(Work in progress)

How it works
------------

(Work in progress)

With this information, the automatic bounce management is possible:

The automatic task `eval_bouncer` gives a score for each bouncing user. The score, between 0 to 100, allows the classification of bouncing users in two levels (level 1 or 2). According to the level, automatic actions are executed periodically by the `process_bouncers` task.

The score evaluation main parameters are:

  - Bounces count:

    The number of bouncing messages received by Sympa for the user.

  - Type rate:

    Bounces are classified depending on the type of errors generated on the user side. If the error type is `mailbox is full` (i.e. a temporary 4.2.2 error type), the type rate will be 0.5, whereas permanent errors (5.x.x) have a type rate equal to 1.

  - Regularity rate:

    This rate tells whether bounces were received regularly, compared to list traffic. The list traffic is deduced from the `msg_count` file data.

The score formula is:
> *Score* = *bounce_count* * *type_rate* * *regularity_rate*

To avoid making decisions (i.e. defining a score) without enough relevant data, the score is not evaluated if:

  - The number of received bounces is lower than [`minimum_bouncing_count`](../man/sympa.conf.5.md#minimum_bouncing_count).
  - The bouncing period is shorter than [`minimum_bouncing_period`](../man/sympa.conf.5.md#minimum_bouncing_period).

Bouncing list member entries expire after a given period of time. The default period is 10 days, but it can be customized if you write a new `expire_bounce` task (see [`expire_bounce_task`](../man/sympa.conf.5.md#expire_bounce_task)).

You can define the limit between each level through the **List configuration panel**, in subsection "[Bounce menagement](../man/list_config.5.md#bouncers_level1)". The principle consists in associating a score interval with a level.

You can also define which action must be applied on each category of user. Each time an action will be performed, a notification email will be sent to the person of your choice.

VERP
----

VERP (Variable Envelop Return Path) is used to ease automatic recognition of subscribers email addresses when receiving a bounce. If VERP is enabled, the subscriber address is encoded in the return path itself, so that the Sympa bounce management process (bounced) will use the address the bounce was received for to retrieve the subscriber email. This is very useful because sometimes, non delivery report do not contain the initial subscriber email address but an alternative address where messages are forwarded. VERP is the only solution to detect automatically these subscriber errors. However, the cost of VERP is significant, indeed VERP requires to distribute a separate message for each subscriber and breaks the bulk emailer grouping optimization.

In order to benefit from VERP and keep the distribution process fast, Sympa enables VERP only for a share of the list members. If [`verp_rate`](../man/sympa.conf.5.md#verp_rate) is `10%`, then after 10 messages distributed in the list all subscribers have received at least one message where VERP was enabled. Later, distribution message enables VERP also for all users where some bounces were collected and analyzed by the previous VERP mechanism.

If VERP is enabled, the format of the messages return path are as follows:
``` code
Return-Path: <bounce+user==a==userdomain==listname@listdomain>
```
Note that you need to set a mail alias for the generic `bounce+*` alias (see ~~[Robot aliases](/manual/mail-aliases#robot_aliases)~~).

ARF
---

ARF (Abuse Reporting Format) is a standard for reporting abuse. It is implemented mainly in the AOL email user interface. AOL servers propose to mass mailer to received automatically the users complain by formated messages. Because many subscribers do not remember how to unsubscribe, they use ARF when provided by their user interface. It may be useful to configure the ARF management in Sympa. It is really simple: all what you have to do is to create a new alias for each virtual robot as the following (Note:
replace [``$IBEXECDIR``](../layout.md#libexecdir) below):

``` code
abuse-feedback-report:       "| $LIBEXECDIR/bouncequeue sympa@mail.example.org"
```

Then register this address as your loop back email address with ISP (for exemple AOL). This way, messages to that email adress are processed by the bounced.pl daemon and automatically processed. If the bounced.pl service can remove a user, the message report feedback is forwarded to the list owner. Unrecognized messages are forwarded to the listmaster.

This feature is available as of Sympa 6.2.4.
