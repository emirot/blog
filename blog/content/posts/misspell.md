---
author: "Nolan"
title: "Check for misspelled words"
date: "2021-04-27"
categories: ["golang", "github"]
draft: false
description: "Testing client9 misspell"
tags: ["blog", "misspell"]
ShowToc: false
TocOpen: false
---

## Checking for typos & misspell words

I came across that project [client9/misspell](https://github.com/client9/misspell) on github that aims to: "Correct commonly misspelled English words... quickly."  
So I decided to try it directly for that blog by using a github action.


## Implementation

Github action:

```yaml
name: Go
on: [push, pull_request]
jobs:
  build:
    name: Check for misspell
    runs-on: ubuntu-latest
    container: golang:1.15-buster
    steps:
    - uses: actions/checkout@v2
    - run: |
         GO111MODULE=on go get -u github.com/client9/misspell/cmd/misspell
         misspell -error misspell blog/content/posts/*.md
```

It worked!
```bash
go: downloading github.com/client9/misspell v0.3.4
go: found github.com/client9/misspell/cmd/misspell in github.com/client9/misspell v0.3.4
blog/content/posts/first_post.md:21:17: "accross" is a misspelling of "across"
blog/content/posts/first_project_golang.md:15:29: "similiar" is a misspelling of "similar"
```

Only gotcha don't forget the `-error` flag so it returns an error code that let you know something is wrong.