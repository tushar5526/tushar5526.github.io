---
title: Pluggy! Implementing plugin system in python
date: "2024-04-29T16:34:25+05:30"
type: "post"
description: "Plugin system for your next python library"
in_search_index: true
tags: ["python", "plugins"]
---

Plugin architecture is quite handy when you are designing a developer-facing library. A lot of times I wanted to allow users to inject their code in some way to extend the core library functions and add domain-specific customizations. I found a great approach while going through [tox](https://github.com/tox-dev/tox) source code and discovered [Pluggy](https://github.com/tox-dev/tox) which builds on top of Python's `entry_point` feature.  I was amazed to know that`entry_point` is not just for CLI, but a clean plugin system that comes with Python. 

In a nutshell, Pluggy provides you with a great interface to define *specs for hooks* in your library that external libraries can register to. At runtime, the registered functions are loaded and called sequentially in LIFO order. 

### References:

- [Anthony Explains - Entry Points and writing your own plugin system.](https://www.youtube.com/watch?v=fY3Y_xPKWNA) 

- [Pluggy's Complete Example](https://pluggy.readthedocs.io/en/stable/#a-complete-example)