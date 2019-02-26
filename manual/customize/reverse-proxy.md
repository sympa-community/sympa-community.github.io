---
title: 'Placing Sympa behind a reverse proxy'
up: ../customize.md#sympa-and-other-systems
---

Placing Sympa behind a reverse proxy
====================================

Apache HTTP Server
------------------

You need to enable the following Apache modules:
  * mod_proxy
  * mod_proxy_http

### Global configuration
 
Once the modules are enabled, you need to customize their configuration.
This can be done in a file global to the reverse proxy server configuiration.

Add the following directives:

```
SSLProxyEngine On

## Most of the time, you need to set these two directives too. see below:
SSLProxyCheckPeerCN off
SSLProxyCheckPeerName off
```
  * `SSLProxyEngine`: enable mod_proxy usage
  * `SSLProxyCheckPeerCN` and `SSLProxyCheckPeerName`, when both set to `off`, 
    will prevent the reverse proxy from checking the remote server certificate's `CN` field.
    This way, you don't have to give the remote the same name as the request URL. That allows to
    have a domain called `lists.example.com` hosted on a server called `sympa.example.com`.
  
### Sympa endpoints configuration

You need to setup three endpoints for Sympa: the web interface (`/sympa` endpoint), the static content
(`/static-sympa` endpoint) and, optionally, the SOAP URL (`/sympasoap` endpoint).

In the following configuration excerpt, `actual.host` is the name of the remote server, the one hosting Sympa.

```
<Location /sympa>
    Require all granted
    RequestHeader set Front-End-Https "On"
    ProxyPass        https://actual.host/sympa
    ProxyPassReverse https://actual.host/sympa
    ProxyPreserveHost On
</Location>

<Location /static-sympa>
    Require all granted
    RequestHeader set Front-End-Https "On"
    ProxyPass         https://actual.host/static-sympa
    ProxyPassReverse  https://actual.host/static-sympa
    ProxyPreserveHost On
</Location>

<Location /sympasoap>
    Require all granted
    RequestHeader set Front-End-Https "On"
    ProxyPass         https://actual.host/sympasoap
    ProxyPassReverse  https://actual.host/sympasoap
    ProxyPreserveHost On
</Location>
```
Note: `ProxyPreserveHost` is mandatory to pass the original host of the request to the Sympa server. Otherwise, your Sympa server
will not know for which virtual host the request is.
