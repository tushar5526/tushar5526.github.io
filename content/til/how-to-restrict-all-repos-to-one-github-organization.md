---
title: "How to Restrict All Repos to One Github Organization"
date: 2021-02-25T16:36:50+02:00
tags:
- all-repos
- github
---

Especially, when maintaining many, many git repositories at once,
Anthony Sottile's [all-repos](https://github.com/asottile/all-repos) is a bliss.

You can easily grep for text or find files in,
or even apply patches to hundreds or thousands of repositories at once,
which I already described in a [blog post](https://jugmac00.github.io/blog/how-to-create-hundreds-of-pull-requests-with-a-single-command/).

`all-repos` has a configuration option,
which allows you to clone and manage all of your GitHub repositories at once.

Mine looks like...

```json
{
    "output_dir": "output",
    "source": "all_repos.source.github",
    "source_settings":  {
        "api_key": "xxx",
        "username": "jugmac00",
        "collaborator": "true",
        "forks": "true",
        "private": "true"
    },
    "push": "all_repos.push.github_pull_request",
    "push_settings": {
        "api_key": "xxx",
        "username": "jugmac00"
    }
}
```

With an `all-repos-clone` I now can download all my personal repos,
all the repos of my organizations (e.g. zopefoundation, plone, ...),
and even the private ones of the company I work for.

In sum these are way more than 1.000 repositories.

But what if you get too many results or just want to search in one GitHub organization,
not in all of them?

## (maybe not so) quick and dirty

Imagine, you want to search only in the GitHub organization of **acme**, instead of running

```bash
all-repos grep search-term
```

do a

```bash
all-repos-grep search-term | grep acme
```

This certainly works, but there some downsides...

1. `all-repos` still searches in all repositories, and thus it can take quite some time.
2. You loose the syntax highlighting of the grep command, which you could counter with...

```bash
all-repos-grep search-term | grep acme | grep search-term
```

## use a separate configuration file

You could create a separate configuration file for `acme` like this...

```json
{
    "output_dir": "output_acme",
    "source": "all_repos.source.github_org",
    "source_settings":  {
        "api_key": "xxx",
        "org": "acme"
    },
    "push": "all_repos.push.github_pull_request",
    "push_settings": {
        "api_key": "xxx",
        "username": "jugmac00"
    }
}
```

and use search like this...

```bash
all-repos-grep -C all-repos-acme.json search-term
```

## create a pull request for a command line option

Maybe Anthony would accept a pull request to add a command line option like e.g. "--in-repo" or "--in-repos",
where you could specify the repository or repositories to search in?

Who knows :-)


## thanks

Many thanks to [Anthony](https://twitter.com/codewithanthony) for creating this and many other invaluable tools,
and many thanks for being helpful on so many occasions.
