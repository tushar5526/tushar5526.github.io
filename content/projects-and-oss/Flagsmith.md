---
title: "Flagsmith"
type: "post"
in_search_index: true
tags: ["oss", "feature-flags", "django", "python"]
---

[Flagsmith](https://github.com/Flagsmith/flagsmith) is an open source, fully featured, Feature Flag and Remote Config service. Use our hosted API, deploy to your own private cloud, or run on-premise. (copy-pasted straight from their GitHub)

It's quite fun to contribute there majorly due to high quality reviews. 

-----------

> **[Migration to Poetry #2214](https://github.com/Flagsmith/flagsmith/pull/2214)**

In this PR, I worked on migrating the entire project from plain old `requirements.txt` to using `poetry`. There were a lot of learnings and you can go through the comments to pick the best practices when using `poetry`. 

-----------

> **[Implementing backoffs in webhooks using Flagsmith's custom task processor](https://github.com/Flagsmith/flagsmith/pull/2932)**

What I believed to be a small feature, turned out to be a pretty big PR. Flagsmith has built their own simpler version of `celery` for task processing. This PR deals with adding backoff to webhooks using the task processor. 

-----------

> **[Adding type hints to Python Client SDK](https://github.com/Flagsmith/flagsmith-python-client/pull/71)**

This PR dealt with adding type hints to the whole python SDK. Acceptance criteria was to make CI pass on `mypy --strict .` 

-----------

> **[Other bug fixes and smaller feature implementations](https://github.com/Flagsmith/flagsmith/issues?q=author%3Atushar5526+)**

Some good first issues I picked around bug fixes and minor feature development. 