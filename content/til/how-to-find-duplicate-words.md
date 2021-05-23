---
title: "How to Find Duplicate Words"
date: 2021-03-24T11:05:58+02:00
tags:
- grep
- ripgrep
---

When contributing to a new open source project,
from time to time I searched the codebase for occurrences of `the the`.

This is a common mistake in comments in English codebases.

My friend [Miroslav](https://twitter.com/eumiro) came up with an even better way:

Use a regex to find duplicate words!

```bash
rg --pcre2 "\b(\w+)\s+\1\b"
```

`rg` stands for [ripgrep](https://github.com/BurntSushi/ripgrep),
which is a blazing fast implementation of a regex command line tool,
written in Rust.

When trying to understand the above regex,
I found an interesting [StackOverflow](https://stackoverflow.com/questions/2823016) question,
where an alternative regex was mentioned,
which even handles words with apostrophes, hyphens...

```bash
rg --pcre2 "(\b\S+\b)\s+\b\1\b"
```
The above link to StackOverflow also explains the regex expression.
