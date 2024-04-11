---
title: "p5py"
type: "post"
in_search_index: true
tags: ["oss", "graphics", "skia", "python", "fellowship"]
---

[p5](https://github.com/p5py/p5) is a Python library that provides high level drawing functionality to help you quickly create simulations and interactive art using Python. It combines the core ideas of Processing.

**PS:** Let me know if you want to help maintain this project! 

---------------

> **[Refactoring of code to support single pattern to support multiple renderers at runtime](https://github.com/p5py/p5/pull/288)**

My first exposure to implementing singleton design pattern. This PR basically dealt with refactoring the code base to remove dependency on Vispy specific APIs.  

---------------

> **[Implemented visual testing by doing sanity checks](https://github.com/p5py/p5/pull/318)** 

This was interesting problem to figure out as I had to use virtual buffers to enable visual testing on GitHub Actions, and some interesting work arounds to run different sketches on run-time. 

---------------

> **[#377](https://github.com/p5py/p5/pull/377)**

In this one, I had to use binary search (finally) in typography PR, to implement text-wrap functionality.  

---------------

> **[Other PRs & Issues on p5py](https://github.com/p5py/p5/issues?q=author%3Atushar5526+)**

I am looking for co-maintainers to better maintain the project. If this excites you - do reachout. 


---------------

> **[Here is the Product Work Report summarizing my stint as a Processing fellow](https://github.com/p5py/p5/blob/master/project-wrapups/summer-fellow-2022-wrapup/tushar-processing-summer-fellow-2022-product-work-report.md)** 