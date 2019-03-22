---
title: 'TLS client authentication'
up: ../customize.md#web-interface-optional-features
---

TLS client authentication
=========================

See also "[Authentication on web interface](authentication-web.md)".

Sympa web interface (WWSympa) provides an
[authentication mechanism](authentication-web.md#authentication-mechanisms)
based on X.509 certificates installed in users' browser.

HTTP server supporting HTTPS (HTTP over TLS) connections provides the
required authentication information through CGI environment variables. You
will need to configure HTTP server to allow HTTPS access and require X.509
client certificate.  No additional setting is needed on the side of Sympa.

Requirements
------------

  - TLS support with HTTP server, for example mod_ssl with
    Apache HTTP Server.

  - [Crypt-OpenSSL-X509](https://metacpan.org/release/Crypt-OpenSSL-X509)
    Perl module.

Configuring HTTP server
-----------------------

### Apache HTTP Server with mod_ssl

``` code
SSLEngine on
SSLVerifyClient optional
SSLVerifyDepth  10
...
<Location /sympa>
    SSLOptions +StdEnvVars +ExportCertData

    ...

</Location>
```

### nginx

``` code
server {
    ...

    ssl_verify_client optional;
    ssl_client_certificate <<a file including trusted CA certificate(s)>>;
    #ssl_verify_depth 1;

    location /sympa {
        ...

        fastcgi_param SSL_CLIENT_VERIFY $ssl_client_verify;
        fastcgi_param SSL_CLIENT_CERT $ssl_client_raw_cert;
    }

    ...
}
```
