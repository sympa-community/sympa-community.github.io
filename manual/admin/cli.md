---
title: 'Command line interface'
prev: services.md
up: ../admin.md
next: list.md
---

Command line interface
======================

Sympa provides several command line management utilities.  See following
reference manuals for more details.

Administration tools
--------------------

  - [`sympa`](/gpldoc/man/sympa.1.html) or `sympa.pl`

    Command line utility to manage Sympa.

    > **Note**
    >
    > On Sympa prior to 6.2.68, most distributions use only the name
    > `sympa.pl`. On 6.2.68 or later, `sympa.pl` is a symbolic link to
    > `sympa`.

  - [`sympa_newaliases.pl`](/gpldoc/man/sympa_newaliases.1.html)

    Alias database maintenance.

> **Note**
>
>   * `sympa_wizard.pl` command line utility to help performing Sympa
>     initial setup was deprecated on Sympa 6.2.70 and part of its
>     functionalities were replaced with
>     [`sympa config`](/gpldoc/man/sympa-config.1.html) command.
>     The manual page of `sympa_wizard.pl` for earlier versions is
>     [here](/gpldoc/man/sympa_wizard.1.html).
