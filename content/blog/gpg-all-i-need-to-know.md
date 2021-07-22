---
title: "GPG - All I Need to Know"
date: 2021-07-22T16:40:59+02:00
---

While I need to use GPG pretty regularly,
I always have to look up the commands - they just don't stick :-)

Over the last couple of months I collected every command I had to use. Enjoy!

## encrypt a file

```bash
gpg --encrypt --recipient someone@example.com filename
```

## decrypt a file

```bash
gpg --decrypt -outfile filename filename.gpg
```

## show all local keys

```bash
gpg --list-keys
```

## search and add a new key

```bash
gpg --keyserver keys.openpgp.org --search-key someone@example.com
```

## import a public key from the file system

```bash
gpg --import key.asc
```

## upload a key

```bash
gpg --send-keys key-id
gpg --keyserver keys.openpgp.org --send-keys key-id
```

## extend the key expiration date

```bash
gpg --edit me@example.com
>expire
...
```

## feedback

Did I miss anything? What commands do you regularly use?
