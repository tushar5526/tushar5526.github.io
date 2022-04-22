---
title: "How to Deactivate Auto Import in Pylance"
date: 2020-06-06T09:14:02+02:00
tags:
- pylance
- vscode
---

Unfortunately, the **auto-import** feature in PyLance surprised me with `random` imports.

e.g. with `from unittest.case import expectedFailure` just when I typed `assert result == expected`.

Luckily, the developers [listened to the users](https://twitter.com/jugmac00/status/1285485995023097856),
and with version 2020.8.0 this "feature" is optional.

In order to deactivate it, set the following option to `false`:

```
python.analysis.autoImportCompletions
```

Thank you, Savannah!
