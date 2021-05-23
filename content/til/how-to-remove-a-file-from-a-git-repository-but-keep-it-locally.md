---
title: "How to Remove a File From a Git Repository but Keep It Locally"
date: 2020-08-10T09:00:14+02:00
tags:
- git
---

For a video project,
I have a video folder with videos and subtitle files.

*Obviously*,
I do not want to have hundreds of megabytes in my git repository,
so I git ignored them - but I committed the subtitles.

Now,
the videos and the subtitles will be served by nginx from a media directory,
outside of the project.

In order to delete the subtitles from the git repository,
but keep them locally, I have to...

```bash
git rm -- cached somefile.ext
```

This also works for directories...

```bash
git rm --cached -r somedir
```

via https://stackoverflow.com/questions/3469741/remove-file-from-the-repository-but-keep-it-locally
