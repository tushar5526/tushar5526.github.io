---
title: "How to Surround Highlighted Text With a Custom Snippet"
date: 2021-08-11T13:17:09+02:00
tags:
- vscode
- flask
- flask-wtf
- jinja
---

In order to mark strings as "translatable" in Flask via Flask-WTF, you need to apply a special syntax.

This means, e.g. in a Jinja template a string like `Conference` has to be transformed into `{{ _('Conference') }}`.

This is a very tedious work, so I created a shortcut for it.

Add the following lines to your `keybindings.json`:

    "key": "ctrl+shift+alt+0",
    "command": "editor.action.insertSnippet",
    "when": "editorHasSelection || editorHasMultipleSelections",
    "args": {
        "snippet": "{{ _('${TM_SELECTED_TEXT}') }}"
    }

I used "ctrl+shift+alt+0" as on keyboard with a German layout the closing curly brace is on the same key as the `0`.

via https://stackoverflow.com/a/53545200/672833
