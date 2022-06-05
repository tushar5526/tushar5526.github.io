---
title: "GPG - All I Need to Know"
date: 2021-07-22T16:40:59+02:00
---

While I need to use GPG pretty regularly,
I always have to look up the commands - they just don't stick :-)

Over the last couple of months I collected every command I had to use. Enjoy!

## create a key

```bash
gpg --quick-generate-key user-id

# or

gpg --generate-key

# or

gpg --full-generate-key
```

## delete a private key

```bash
gpg --delete-secret-keys key-id
```

## delete a public key

```bash
gpg --delete-key key-id
```

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

## revoke a key

```bash
# always do this and keep it safely
gpg --output revoke.asc --gen-revoke key-id

# do this when you actually need to revoke it
gpg --import revoke.asc
gpg --keyserver keys.openpgp.org --send-keys key-id
```


## feedback

Did I miss anything? What commands do you regularly use?

## update

### 2022-06-05

- added command to create a new key
- added commands to delete private and public keys
- added commands to revoke a key
