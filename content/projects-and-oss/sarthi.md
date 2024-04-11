---
title: "Sarthi"
type: "post"
in_search_index: true
tags: ["ci/cd", "docker", "nginx", "docker-compose", "python"]
---

> **Vercel for Backend! Easily setup preview environments with just Docker**

I really wanted to have a tool that can enable preview environments for my projects without much husstle. I was majorly looking for something that can take in a `docker-compose` file at the root of project and gives you preview environments right out of the box, that can be self hosted on your server. 

This project uses a lot of other OSS projects to export logs, manage secrets, monitoring and for admin actions. Check it out at [**Sarthi**](https://github.com/tushar5526/sarthi). It is meant to be used along with [**sarthi-deploy**](https://github.com/tushar5526/sarthi-deploy) GitHub Action for setting up preview environments in your project. Every time there is a new branch or a PR created, Sarthi GHA will create a preview environment for that. It also takes care of deleting preview environments when respective branches or PRs are merged.

**PS:** If you are thinking where the name comes from, **Sarthi** in **Hindi** means a companion who can help steer someone else on their journey. This project helps dev in their project development journey and hence the name!
