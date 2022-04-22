---
title: "How to Cargo Install a Package With Additional Features"
date: 2022-04-22T08:04:54+02:00
tags:
- rust
- ripgrep
- cargo
---

Meanwhile you may have heard that there are a
[couple of new CLI apps](https://jvns.ca/blog/2022/04/12/a-list-of-new-ish--command-line-tools/)
out there written in Rust - blazing fast and with additional features.

While I rarely use them, as I do not want to get used to tools,
which are not available on all Linux machines I work on,
I do use [ripgrep](https://github.com/BurntSushi/ripgrep) regularly,
as it really makes a difference,
especially in speed.

Yesterday, I wanted to use ripgrep with PCRE2 (Perl Compatible Regular Expressions),
but when I tried it, I got this error message:

```bash
$ rg --pcre2 "(\b\S+\b)\s+\b\1\b"
PCRE2 is not available in this build of ripgrep
```

When I googled for the error message, I found the
[FAQ](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#how-do-i-use-lookaround-andor-backreferences)
of the ripgrep project, which said...

> If you installed ripgrep through a different means,
> then please reach out to the maintainer of that package to see
> whether it's possible to enable the PCRE2 feature.

Ok, let's see where I got ripgrep from.

```bash
$ which rg
/home/jugmac00/.cargo/bin/rg
```

Oops, I installed it myself.

I do had a look at ripgrep's documentation, and some Rust documentation,
and while I found out I could build a project with additional features like this...

```bash
$ cargo build --release --features 'pcre2'
```

... this would not solve my problem as then I have a binary which is not on my path.

### solution

Finally I asked on [ripgrep's discussion board](https://github.com/BurntSushi/ripgrep/discussions/2190)
and immediately got an answer:

```bash
$ cargo install --features 'pcre2' ripgrep
```
