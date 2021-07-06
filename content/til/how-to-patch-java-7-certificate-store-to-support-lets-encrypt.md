---
title: "How to Patch Java 7 Certificate Store to Support Let's Encrypt"
date: 2021-07-06T09:58:39+02:00
tags:
- java
- ssl
- tls
- letsencrypt
---

Do you have to support a very old Java application?

Old as in *only runs on 1.7.0_21-b11*?

And this application needs to access websites on servers using [Let's Encrypt](https://letsencrypt.org/)?

Especially after [September 2021](https://letsencrypt.org/docs/dst-root-ca-x3-expiration-september-2021/),
when the widespread **DST Root CA X3** certificate will expire?

There is help.

## keytool to the rescue

Oracle kindly provides [keytool](https://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html).

With keytool you can view and manipulate the contents of the Java certificate store,
which usually can be found at `/lib/security/cacerts` within in your Java runtime.

## view certificates

```bash
keytool -list -keystore cacerts -storepass changeit
```

...where you need to replace
- `cacerts` with the path to your certificate store location
- `changeit` with the password for the store (`changeit` is the default password)

## add new certificates

First, you need to download the new `ISRG Root X1/X2` certificates
from [Let's Encrypt's website](https://letsencrypt.org/certificates/).

Then you just have to add them to your store ...

```bash
keytool -import -keystore cacerts -storepass changeit -noprompt -trustcacerts -alias isrgrootx1.der -file isrgrootx1.der
```

## test the updated store

Let's encrypt kindly offers a test page to verify your changes worked: https://valid-isrgrootx1.letsencrypt.org/

## further information

- https://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
- https://letsencrypt.org/docs/dst-root-ca-x3-expiration-september-2021/
- https://letsencrypt.org/certificates/
- https://valid-isrgrootx1.letsencrypt.org/
