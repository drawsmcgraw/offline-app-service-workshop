# Certificate Authentication

The platform you're using should be configured terminate TLS connections at the GoRouter (a component of the Platform). This happens before any traffic reaches your application. What this means for you is that you, as the developer, do not need to be concerned with Certificate Authority certs, chains, or handing authentication. If a user makes it to your application, they have already been verified to be a legitmate user (i.e. authenticated) by the platform.

This configuration is illustrated in the image below.

![TLS terminates at the GoRouter](/docs/img/tls-pass-through.png)

After handling the TLS termination and user authentication, the Platform will take the PEM-encoded x509 user certificate and place it into the `xfcc` (x-forward-client-cert) HTTP header. If you are using Spring, you can take advantage of Spring Security to automatically parse this certificate. Even without Spring, you can easily find this cert in the xfcc header.

We're going to test this capability in this exercise with another sample application. This is a simple application that describes its envirionment (i.e. environment variables) as well as dumps all HTTP headers for easy human inspection.

Download the app [with this link](/docs/pkg/cf-env.tgz)

No building/compiling is necessary. Just unpack, `cd` into the `cf-env` directory, and `cf push` with the existing `manifest.yml`.

After pushing, take the route that the Platform gives you and put it into your browser (remember to use `https` when putting the route into your browser). Once the app loads in your browser, click the "Headers" tab and confirm that the contents of the `X-Forwarded-Client-Cert` header are, indeed, the contents of your cert (i.e. what you loaded into your browser and chose to send to the app).

The below example of what to expect.

```
X-Forwarded-Client-Cert:
MIIFYTCCA0mgAwIBAgIQEHRAqT9wgUUD/YM/AhSBlTANBgkqhkiG9w0BAQsFADA2MRUwEwYDVQQKEwxwd3MgZGFyayBkZXYxHTAbBgNVBAMTFHB3cyBkYXJrIGRldiBSb290IENBMB4XDTIwMTAxNTIxNDcxMloXDTIxMTAxNTIxNDcxMlowSzEeMBwGA1UEChMVcHdzIGRhcmsgZGV2IFVzZXIgUEtJMQ8wDQYDVQQLEwZQZW9wbGUxGDAWBgNVBAMTD1Ntb2tlIFRlc3QgVXNlcjCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCCAgoCggIBAJsmQKstaPvcM6l
```

This concludes the Cert Auth section of the workshop.
