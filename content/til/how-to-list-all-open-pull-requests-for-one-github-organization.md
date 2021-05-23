---
title: "How to List All Open Pull Requests for One GitHub Organization"
date: 2021-04-19T09:03:29+02:00
tags:
- github
- pull-requests
---

While most developers probably know,
that you can list all your open pull requests on GitHub via https://github.com/pulls,
how can you list all open pull requests for one GitHub organization?

I was on holiday for two weeks,
and I want to get up to speed which pull requests need to be reviewed in my company...

Luckily, there is no need to do a complicate query or work with GitHub's GraphQL API...

Just replace "ORG" with your organization and click on the following link...

https://github.com/pulls?user=ORG

Alternatively, you can use the GitHub search, either by clicking the top left search field or entering "/" and entering the following query:

```
is:pr is:open org:ORG
```

##Update
- use an even simpler URL
- show an alternative way - thank you, Anthony Sottile!
