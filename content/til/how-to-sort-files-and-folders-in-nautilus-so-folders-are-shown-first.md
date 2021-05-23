---
title: "How to Sort Files and Folders in Nautilus So Folders Are Shown First"
date: 2020-08-25T09:07:30+02:00
tags:
- linux
- nautilus
---


## update settings

```
gsettings set org.gtk.Settings.FileChooser sort-directories-first true
```

## check settings

```
gsettings get org.gtk.Settings.FileChooser sort-directories-first
```

via https://askubuntu.com/questions/1064482/
