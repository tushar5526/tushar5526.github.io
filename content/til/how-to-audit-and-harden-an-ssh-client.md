---
title: "How to Audit and Harden an SSH Client"
date: 2020-07-22T11:30:28+02:00
tags:
- ssh
- security
- audit
---

You probably know how to harden a SSH server,
or at least heard of it.

e.g. do not offer weak ciphers, or do not allow root login...

But did you know you can and also should harden your SSH client?

## step 1 - auditing your SSH client

### terminal 1

```bash
git clone https://github.com/jtesta/ssh-audit

cd ssh-audit

python3.8 ssh-audit.py -c  # c = client audit; this starts a ssh server on port 2222
```

### terminal 2

```bash
ssh localhost -p 2222
```

Now, switch back to terminal 1 and have a look at the output - it all should be green - but it won't.


## step 2 - hardening your SSH client

Similar to a server,
you have to restrict which **Ciphers**, **KexAlgorithms**... you accept.

But which **Ciphers**... are ok?

Luckily, there is a security expert out there,
and he does the hard work, see:

https://www.ssh-audit.com/hardening_guides.html


But... echoing the **Cipher**... configurations did not change anything for me... and this took a while to figure out why.

Turns out, in your `~/.ssh/config` file you have to put all configuration options above your `Host` configurations,
otherwise the options only apply to the last host!

`man 5 ssh_config`

>  Host    Restricts the following declarations (up to the next Host or Match keyword) to be only for those hosts that match one of the patterns given after the keyword.

TIL: **man pages** are better than their reputation :-)

## further resources

https://www.positronsecurity.com/blog/2020-01-07-ssh-client-auditing-and-hardening/
