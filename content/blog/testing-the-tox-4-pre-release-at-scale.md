---
title: "Testing the tox 4 Pre-Release at Scale"
date: 2021-03-01T15:31:06+02:00
---

Every once in a while,
you may read that one of your favorite used packages announces a new version.

Sometimes even a so-called alpha version is announced.

The maintainers then politely ask you, the user, to test the package,
and give feedback if anything is broken.

When the upcoming [pytest](https://docs.pytest.org/en/stable/) version 6 had been announced,
I wrote a short instruction on how to
[install pre-releases](https://github.com/jugmac00/til/blob/master/python/install-release-candidates.md).

This time, [tox](https://tox.readthedocs.io/en/latest/),
the **virtualenv** management and test tool,
announced a new version 4, but it is not just a new version,
it is a complete rewrite by [Bernát Gábor](https://twitter.com/gjbernat)!

So, if you use **tox**, and you certainly should,
testing your package with **tox 4 alpha** is highly recommended.

Stop reading now, and try it - I am serious!

I did test an early alpha version,
and found quite a couple of issues for the projects I maintain (
[1](https://github.com/tox-dev/tox/issues/1781)
[2](https://github.com/tox-dev/tox/issues/1782)
[3](https://github.com/tox-dev/tox/issues/1783)
[4](https://github.com/tox-dev/tox/issues/1804)
[5](https://github.com/tox-dev/tox/issues/1859)
[6](https://github.com/tox-dev/tox/issues/1868)
).

They were all addressed in no time.

Reporting issues in pre-releases is important both for you,
so you can use the new version once it gets released without any hassle,
and also for the maintainer,
who does not get stressed out completely on the day the new package gets released.

End of story...

Not yet!

## alpha testing on scale

Maybe you know that I am contributor to the [Zope project](https://zope.readthedocs.io/en/latest/).
Zope is the bedrock of Python web frameworks.
And while development has come to a *sustainable* path, it is still used out in the wild.

One characteristic, which I miss in other projects, is
that not only the core project,
but also hundreds and hundreds of plugins are managed together in one [GitHub organization](https://github.com/zopefoundation).

I mentioned the advantages already a couple of times.

All contributors can access all packages, fix minor issues,
which means no package is left behind,
and no package depends on the good will of a single maintainer,
who always starts euphoric,
and at one point in time neglects the project because of real life or switching company or language.

Ok, so we use **tox** in most of the roundabout 300 active packages.

How would I be able to run **tox alpha** on all of them?

## all-repos to the rescue

I already wrote about [all-repos](https://github.com/asottile/all-repos)
by [Anthony Sottile](https://twitter.com/codewithanthony) previously in
[How to create hundreds of pull requests with a single command?](https://jugmac00.github.io/blog/how-to-create-hundreds-of-pull-requests-with-a-single-command/),
but this time it is different.

If you have not encountered **all-repos** yet...

In short, with **all-repos** you can clone all of your and your organization's repositories,
find files,
grep in them,
mass apply changes to them and finally create pull requests,
all from the command line.

But afaik **all-repos** does not offer an easy way to run a custom command on all repositories.

Also, unlike e.g. **pytest**, it also does not offer any plugin mechanism, but ...

It is super easy to use **all-repos** as a library.

## use the source

While the [README](https://github.com/asottile/all-repos/blob/master/README.md) offers a nice overview of the API,
I directly dug into the source code to find some inspiration on how to write my custom **all-repos** command line script.

**all-repos** offers a couple of command line entry points
and also a couple of [example usages](https://github.com/asottile/all-repos/tree/master/all_repos/autofix),
and after looking around a bit,
I chose the script for [all-repos-list-repos](https://github.com/asottile/all-repos/blob/master/all_repos/list_repos.py) as a template,
which basically does everything I wanted,
except executing **tox**.

So, **all-repos** exposes a `load_config` method,
with which I get hold of all repositories,
and instead of just printing their paths as in `all-repos-list-repos`,
I need to check whether they contain a **tox.ini** file,
and if so, run **tox4 alpha**.

After that I check the return code of **tox**.
If the run succeeds, and it does for **tox3** otherwise our CI would not be green,
all is ok, otherwise I need to collect the repo name for later inspection.

At the end I just print the result.

### first version of the script

```python
def main(argv: Optional[Sequence[str]] = None) -> int:
    parser = argparse.ArgumentParser(
        description='Run tox4 on all cloned repositories.',
        usage='python main.py -C configfile',
    )
    cli.add_common_args(parser)
    cli.add_output_paths_arg(parser)
    args = parser.parse_args(argv)

    config = load_config(args.config_filename)
    results = {"notox": [],
               "successful": [],
               "problems": [],
    }
    for i, repo in enumerate(config.get_cloned_repos()):
        path_repo = os.path.join(config.output_dir, repo)
        path_tox = os.path.join(path_repo, "tox.ini")
        if os.path.exists(path_tox):
            print(f"about to run tox for {repo}, {i+1} of {len(config.get_cloned_repos())}")
            run = subprocess.run(["tox4", "-e py39", "-c", path_tox], capture_output="True")

            if run.returncode == 0:
                print(f"tox4 run successful for {repo}")
                results["successful"].append(repo)
            else:
                print(f"tox4 run failed for {repo}")
                results["problems"].append(repo)

        else:
            results["notox"].append(repo)
            print(f"{repo} does not contain a tox configuration. Boo!")
    print(results)
    return 0
```

The latest version of the script is [available on GitHub](https://github.com/jugmac00/my-all-repos).

The end...?

Not yet...

## real life issues

Super easy, right?

Yep, except after quite some time, my Ubuntu machine got completely unresponsive.

Not even keyboard presses gave any reaction...

After a hard reset, I inspected **syslog**,
and found out that my machine ran out of memory and started killing processes.

But why?

I had another look at my script and still had no clue.

I asked at the [tox discord channel](https://discord.gg/tox),
and both Anthony and Bernát gave me some valuable tips on how to improve my code.

I ran my script again,
and after quite some time it made boom again.

Finally, with the help of a system monitoring tool,
I pinpointed the problem to a single repository,
which for some yet to be investigated reasons,
started an endless **setuptools** update circle and ate memory like crazy.

After I excluded this one repository,
my script finally completed,
and showed me the result.

> tox 4 alpha 6 on 288 Zope repositories, 17 without tox, 62 successful, 208 broken builds

Bernát's reaction:

> "a 22% success here, not that great news :smile: but on plus side seems if we fix zope we should be in a decent place"

This was exactly my intention! :-)

P.S.: By the way, there are certainly not 208 different errors / bugs,
there are quite some repeating errors,
which I will have a look at myself at first,
and when it turns out this is not a problem in the package,
I will report an issue in the issue tracker of tox.

## thanks

Thanks to Anthony and Bernát for all your work in the Python open source eco system,
and for always being friendly and helpful!

## found and reported issues

- [`package init file missing` warning when running tox4](https://github.com/zopefoundation/zope.testrunner/issues/112)
- [tox4: fails when requirements file contains a line a la `-e .[test] -c constraints.xt`](https://github.com/tox-dev/tox/issues/1929)
- [tox is broken #24](https://github.com/plone/plone.memoize/issues/24)
- [tox4: tox fails to parse tox.ini when a testenv contains `deps = .[test]`](https://github.com/tox-dev/tox/issues/1933)
- [tox broken #114](https://github.com/plone/plone.app.standardtiles/issues/114)
- [tox broken #14](https://github.com/plone/plone.reload/issues/14)
- [tox will break with the release of tox4 #18](https://github.com/plone/Products.ExtendedPathIndex/issues/18)
- [tox run broken #80](https://github.com/plone/diazo/issues/80)
- [tox broken: "ImportError: No module name `zope.testrunner` #74](https://github.com/plone/plone.app.testing/issues/74)
- [tox configuration is Python 2.7 only #6](https://github.com/plone/plone.gallery/issues/6)
- [tox4: ValueError: No closing quotation (via shlex) #1944](https://github.com/tox-dev/tox/issues/1944)
- [tox4: editable installs do not work (deps) #1945](https://github.com/tox-dev/tox/issues/1945)
- [tox will break when tox4 will be released #48](https://github.com/zopefoundation/Products.PythonScripts/issues/48)
- [tox4: a possible quoting issue #1948](https://github.com/tox-dev/tox/issues/1948)
- [tox is broken: couldn't open ...buildout.cfg #52](https://github.com/zopefoundation/zopetoolkit/issues/52)
- [no support for Python 3.8 - module `cgi` has no attribute `escape` #8](https://github.com/zopefoundation/keas.kmi/issues/8)
- [ResourceWarning: Enable tracemalloc to get the object allocation traceback](https://github.com/zopefoundation/bobo/issues/18)


## updates

**2021-04**
- Meanwhile something crazy happened - Bernát asked me to join the tox maintainer team!
If you ever asked yourself, how would you become an open source maintainer,
just start helping!
