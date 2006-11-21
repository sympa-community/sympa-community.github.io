Sympa RSS channel
=================

This service is provided by *WWSympa* (*Sympa*'s web interface). Here is the root of *WWSympa*'s rss channel :

(Default value: `http://<`host`>`/wws/rss)
Example: `https://my.server/wws/rss`

The access control of RSS queries proceed on the same way as *WWSympa* actions referred to. *Sympa* provides the following RSS features :

  - the latest created lists on a robot (`latest_lists`) ;
  - the most active lists on a robot(`active_lists`) ;
  - the latest messages of a list (`active_arc`) ;
  - the latest shared documents of a list (`latest_d_read`) ;


`latest_lists`
--------------

This provides the latest created lists.

Example: `http://my.server/wws/rss/latest_lists?for=3&count=6`
This provides the 6 latest created lists for the last 3 days.

Example: `http://my.server/wws/rss/latest_lists/computing?count=6`
This provides the 6 latest created lists with topic \`\`computing''.

Parameters :

  - `for` : period of interest (expressed in days). This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.
  - `count` : maximum number of expected records. This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.
  - topic : the topic is indicated in the path info (see example below with topic computing). This parameter is optional.


`active_lists`
--------------

This provides the most active lists, based on the number of distributed messages (number of received messages).

Example: `http://my.server/wws/rss/active_lists?for=3&count=6`
This provides the 6 most active lists for the last 3 days.

Example: `http://my.server/wws/rss/active_lists/computing?count=6`
This provides the 6 most active lists with topic \`\`computing''.

Parameters :

  - `for` : period of interest (expressed in days). This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.
  - `count` : maximum number of expected records. This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.
  - topic : the topic is indicated in the path info (see example below with topic computing). This parameter is optional.


`latest_arc`
------------

This provides the latest messages of a list.

Example: `http://my.server/wws/rss/latest_arc/mylist?for=3&count=6`
This provides the 6 latest messages received on the mylistlist for the last 3 days.

Parameters :

  - list : the list is indicated in the path info. This parameter is mandatory.
  - `for` : period of interest (expressed in days). This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.
  - `count` : maximum number of expected records. This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.


`latest_d_read`
---------------

This provides the latest updated and uploaded shared documents of a list.

Example: `http://my.server/wws/rss/latest_d_read/mylist?for=3&count=6`
This provides the 6 latest documents uploaded or updated on the mylistlist for the last 3 days.

Parameters :

  - list : the list is indicated in the path info. This parameter is mandatory.
  - `for` : period of interest (expressed in days). This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.
  - `count` : maximum number of expected records. This is a CGI parameter. It is optional but one of the two parameters `for` or `count` is required.

