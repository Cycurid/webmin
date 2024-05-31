# CycurID BFA integration into Webmin

This document needs to be updated with the purpose to integrate CycurID BFA in to the webmin list of providers for 2FA.

===
## TODO

- [ ] Create all the language files
- [ ] 



### Create language files in twofactor.lang.auto.html/twofactor.lang.html

Need to add for content to the language files base don the below template:

```
<dt><b>Google Authenticator</b>
<dd>This is a smartphone app that implements the standard TOTP protocol. Each
    user must scan a QR code using the app to link their tokens with the
    Webmin server. <p>
```