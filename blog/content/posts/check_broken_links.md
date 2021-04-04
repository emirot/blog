---
author: "Nolan"
title: "Check broken links in Markdown"
date: "2021-03-03T20:35:29-07:00"
categories: ["blog", "hugo", "markdown"]
draft: false
description: "First blog post on my new blog!"
tags: ["hugo", "blog", "links", "markdown"]
ShowToc: false
TocOpen: false
---

# Broken links

It's always frustrating to follow a dead link, so I would like to avoid that on my blog.

I've found couple of projects that should help:
- https://github.com/raviqqe/muffet
- https://github.com/linkchecker/linkchecker
- https://github.com/stevenvachon/broken-link-checker

These projects looks to be a good fit, however I would have adapt the CI part.
If not I would be checking my current online version and not the latest changes, one idea could be to spin up a local version and test again that one. Or I could do deploy on Pull Request for example.


# Solution

There is a simpler way: https://github.com/gaurav-nelson/github-action-markdown-link-check
The posts are written in markdown so it will check all the posts, so it's not checking everything but the most important!  
Moreover, it's super simple to add I just have to copy/paste an already existing github action.

```bash
name: Check Markdown links
on: push
jobs:
  markdown-link-check:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - uses: gaurav-nelson/github-action-markdown-link-check@v1
```

Tada! I now have a broken link checker, well does it work.
I've added a broken link in a .md file and the job failed looks good so far :)