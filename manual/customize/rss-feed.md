---
title: 'RSS feed'
up: ../customize.md#customizing-web-interface
---

RSS feed
========

This service is provided by WWSympa (Sympa's web interface). Here is the root of WWSympa's RSS channel: *wwsympa_url*`/rss`,
for example `https://web.example.org/sympa/rss`.

The access control of RSS queries do _not_ proceed on the same way as WWSympa. Indeed, most RSS client do not deliver cookies are are not able to deal with authentication forms or SSO redirection. In addition some popular services are used to share RSS feed and so there is not way to be sure of who is reading a feed. That's why RSS Sympa interface deliver all page as if the user was not authenticated.
You may be disappointed by this restriction if you are using a web browser as RSS reader.

Sympa provides the following RSS features:

  - the latest created lists on a robot (`latest_lists`);
  - the most active lists on a robot(`active_lists`);
  - the latest messages of a list (`latest_arc`);
  - the latest shared documents of a list (`latest_d_read`).

`latest_lists`
--------------

This provides the latest created lists.

Example: `https://web.example.org/sympa/rss/latest_lists?for=3&count=6`
This provides the 6 latest created lists for the last 3 days.

Example: `https://web.example.org/sympa/rss/latest_lists/computing?count=6`
This provides the 6 latest created lists with topic `computing`.

Parameters:

  - `for`: period of interest (expressed in days). This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.
  - `count`: maximum number of expected records. This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.
  - topic: the topic is indicated in the path info (see example below with topic `computing`). This parameter is optional.

`active_lists`
--------------

This provides the most active lists, based on the number of distributed messages (number of messages received).

Example: `https://web.example.org/sympa/rss/active_lists?for=3&count=6`
This provides the 6 most active lists for the last 3 days.

Example: `https://web.example.org/sympa/rss/active_lists/computing?count=6`
This provides the 6 most active lists with topic `computing`.

Parameters:

  - `for`: period of interest (expressed in days). This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.
  - `count`: maximum number of expected records. This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.
  - topic : the topic is indicated in the path info (see example below with topic computing). This parameter is optional.

`latest_arc`
------------

This provides the latest messages of a list.

Example: `https://web.example.org/sympa/rss/latest_arc/mylist?for=3&count=6`
This provides the 6 latest messages received on the *mylist* list for the last 3 days.

Parameters:

  - list: the list is indicated in the path info. This parameter is mandatory.
  - `for`: period of interest (expressed in days). This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.
  - `count`: maximum number of expected records. This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.

`latest_d_read`
---------------

This provides the latest updated and uploaded shared documents of a list.

Example: `https://web.example.org/sympa/rss/latest_d_read/mylist?for=3&count=6`
This provides the 6 latest documents uploaded or updated on the *mylist* list for the last 3 days.

Parameters:

  - list: the list is indicated in the path info. This parameter is mandatory.
  - `for`: period of interest (expressed in days). This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.
  - `count`: maximum number of expected records. This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.

