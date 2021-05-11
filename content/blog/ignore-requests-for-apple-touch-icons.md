---
title: "ignore requests for apple touch icons"
date: 2018-07-28T11:38:38+02:00
---

## the problem

My application's error log is polluted with tracebacks of *notFoundErrors* from Apple browsers requesting *apple-touch-icon.png*s.

For those, who do not know... 
Touch icons are to Apple's mobile devices what [favicons]( https://en.wikipedia.org/wiki/Favicon) are to (desktop) web browsers.

According to [ComputerHope](https://www.computerhope.com/jargon/a/appletou.htm):

> When someone bookmarks your web page or adds your web page to their home screen, this icon is used.

So, when you run a public website, no question,
you want to put *some effort* into making some great icons,
so your website looks great on your users' home screen and bookmark section.

And by *some effort* I mean quite some. I was not able to quickly find out how many icons one has to create currently - for all different screens sizes and resolutions.

If you have to take care of touch icons,
try Apple's [Configuring Web Applications](https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html) as a starting point.

## initial situation

I do not run a public website, but an internal application with a two digit user number.

When one of my colleagues accesses the application with an Apple device, and does "things",
nginx, configured as reverse proxy,
passes the request for the touch icon to the application server, and it makes boom!

At least for the moment,
I do not care about the error messages,
and I just do not want to see them.

## technical solution >> design solution

The easiest way to get rid of those messages,
is NOT to create estimated **18** (update: **40**) differently named and sized touch icons,
but not to pass the incoming requests to the application server.

nginx makes this easy, as all I need to add to the configuration, is:

```nginx
location ~ /apple-touch-icon(|-\d+x\d+)(|-precomposed).png {
    return 404;
    log_not_found off;
}
```

where...
- the *location* directive configures what should happen, depending on the request [URI]( https://en.wikipedia.org/wiki/Uniform_Resource_Identifier )
- *~* means a regex is following

- *apple-touch-icon(|-\d+x\d+)(|-precomposed).png* should match any and more of the following:

```bash
apple-touch-icon.png
apple-touch-icon-precomposed.png
apple-touch-icon-74x74.png
apple-touch-icon-180x180.png
```

- *return 404* sends "page not found" to the user

- *log_not_found off* makes nginx to not log this "error"

Fair enough?


## bonus

Want to be prepared for the glorious time when you introduce touch icons?

Dig into nginx' [try_files directive](http://nginx.org/en/docs/http/ngx_http_core_module.html#try_files),
so nginx looks for a file and delivers it,
and if it's not there, it returns a 404 (and not pass the request to your application server).

Following (untested) configuration snippet should do the trick:

```nginx
location ~ /apple-touch-icon(|-\d+x\d+)(|-precomposed).png {
    try_files $uri =404;
}
```


## further resources

- [Configuring nginx @Digital Ocean](https://www.digitalocean.com/community/tutorials/understanding-nginx-server-and-location-block-selection-algorithms)
- [FAQ @Favicon Generator. For real.](https://realfavicongenerator.net/faq#.W1rgidIzZaQ)