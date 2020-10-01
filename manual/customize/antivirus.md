---
title: 'Antivirus plug-in'
up: ../customize.md#sympa-services-optional-features
---

Antivirus plug-in
=================

Sympa lets you use an external antivirus solution to check incoming mails. In this case you must set the `antivirus_path` and `antivirus_args` configuration parameters (see "[Antivirus plug-in](/gpldoc/man/sympa_config.5.html#antivirus-plug-in)"). Sympa supports at least following products:

  - McAfee/uvscan
  - Fsecure/fsav
  - Sophos
  - AVP
  - Trend Micro/VirusWall
  - [Clam Antivirus](https://www.clamav.net/)

For each email received, Sympa extracts its MIME parts in the [``$SPOOLDIR``](../layout.md#spooldir)`/tmp/antivirus` directory and then calls the antivirus software to check them. When a malware is detected, Sympa looks for the name of it in the standard outout of virus checker and sends a warning message to the sender of the email using `your_infected_msg.tt2` template. The email is saved as "bad" and the working directory is deleted (except if Sympa is running in debug mode).

