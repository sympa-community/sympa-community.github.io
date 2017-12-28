S/MIME and HTTPS authentication
-------------------------------

Chapter [Use of S/MIME signature by Sympa itself](/manual/x509#use_of_smime_signatures_by_sympa_itself) deals with Sympa and S/MIME signature. Sympa uses the `OpenSSL` library to work on S/MIME messages, you need to configure some related Sympa parameters: [S/X509 Sympa configuration](/manual/x509#ssympa_configuration).

Sympa HTTPS authentication is based on Apache+mod\_SSL that provide the required authentication information through CGI environment variables. You will need to edit the Apache configuration to allow HTTPS access and require X509 client certificate. Here is a sample Apache configuration:

``` code
SSLEngine on
SSLVerifyClient optional
SSLVerifyDepth  10
...
<Location /sympa>
    SSLOptions +StdEnvVars
    SetHandler fastcgi-script
</Location>
```

If you are using the SubjAltName, then you additionaly need to export the certificate data because of a `mod_ssl` bug. You will also need to install the textindex Crypt-OpenSSL-X509 CPAN module. Add this option to the Apache configuration file:

``` code
SSLOptions +ExportCertData
```

