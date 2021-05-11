---
title: "Convincing an Enterprisy App to Work Behind nginx as Reverse Proxy"
date: 2020-10-30T15:26:14+02:00
---

For simplicity, let's the call the app **Dated HR**,
a tool to "Simplify your HR work",
which offers support for time tracking, holiday, payroll...

So far so good, and even better,
as it is an enterprisy Windows software,
which needs to be configured with **IIS** and a **MSSQL** database,
a colleague of mine installed it on an internal Windows server.

The app makes a web GUI available under http://nemesis.company.local,
which would work like a charm - for our colleagues at site,
but not for the colleagues from the other sites.

But certainly, it is 2020, you want encryption via https,
and also the other colleagues need direct web access.

Setting up **nginx** as reverse proxy is business as usual.

Also, adding https via **Certbot** can't get much simpler:
https://certbot.eff.org/

So far, so good, the app is now available via
https://hr.company.com

The app seems to work,
but when clicking on a special button
(I refrain from calling it a link,
as it is a humongous JavaScripty-eventlistener-from-data-attribute-fetching something),
I get a ...

`This site cannot be reached" nemesis.company.local’s server IP address could not be found.`

Having a look at the HTML source,
a wild mix of relative and absolute URLs shows up,
where the latter have `http://nemesis.company.local` hardcoded.

If this was an open source app, I could
- read the documentation on the internet
- have a look in the source code
- have a look at the issue tracker
- search on Stack Overflow
- ...

...but... did I mention it is an **enterprisy** app?

So, let's tell the Windows admin to configure the app properly.

Turns out, there is no option.

Asking the vendor for support also did not work out:

> The reported behavior cannot be changed.

Well, maybe you cannot... but I can - possibly.

**nginx**, my favorite web server,
can rewrite HTML on the fly,
via the [http_sub_module](http://nginx.org/en/docs/http/ngx_http_sub_module.html).

While this module is not compiled into **nginx** by default,
luckily the **Ubuntu** developers included it.

So, adding the following snippet to my location block should do the trick...

```nginx
sub_filter 'http://nemesis.company.local' 'https://$host';
sub_filter_once off;
```

where

- the first directive replaces the local host name with the one of the reverse proxy
- and the second one makes sure every occurrence gets replaced

After restarting **nginx**,
I still get the same error. **bummer**.

I double checked,
no more occurrences in the generated HTML, CSS or JavaScript...

Hm, let's have a look at the **HTTP headers** in the debug console.

Here we go, the `location` header of the response had the following value...

```http://nemesis.company.lcoal/.../.../.../.../Default.aspx```

Once more, after some googling,
the **nginx** documentation offered a solution
via the [proxy_redirect directive](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_redirect).

While `proxy_redirect` sounds a bit odd,
the description makes more sense:

> Sets the text that should be changed in the "Location" and "Refresh" header fields of a proxied server response.

Very nice, let's add the following snippet to my location block...

```nginx
proxy_redirect http://nemesis.company.local https://$host;
```

After restarting **nginx** once more,
everything works as intended!

The complete location block looks like...

```nginx
location / {
    proxy_pass http://nemesis.company.local:80/;
    sub_filter 'http://nemesis.company.local' 'https://$host';
    sub_filter_once off;
    proxy_redirect http://nemesis.company.local https://$host;
    }
```

Finally, the enterprisy app works like a charm - thanks to **open source**!

P.S.: While debugging the errors,
I found several seriously outdated JS libraries with some known **CVE** entries.

Looking forward to the vendor's answer!

At very least the developers of this very app show good humor,
as they named the variables after cocktails,
which I can relate to.

Cheers! Prost! Na zdraví!
