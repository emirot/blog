---
author: "Nolan"
title: "Using GOPROXY"
date: "2021-04-19"
categories: ["golang", "gproxy"]
draft: false
description: "Getting dependencies via goproxy"
tags: ["golang", "goproxy",]
ShowToc: false
TocOpen: false
---

## Getting dependencies via GOPROXY

I recently wanted to use a private registry in a go project.  

It can be for many reasons:
- ensure immutability from one built to another
- ensure avaibility of a build (remember npm author who removed his library and broke half the internet?)
- get non-public library

## How it works
### Quick & Easy
It is super easy you just have to add an env variable:

```bash
export GOPROXY=export GOPROXY="myprivateserver.com/registry/api/go/go
```

You can then do a:

```bash
go env
```

To see if it was applied.

### Does it really work?

Then I was curious to know if it really worked or not?

I tried verbose but it wouldn't show me the registry.  
I then tried:
```bash
go get -v -t  github.com/opentracing/opentracing-go@fakeversion
```
But it will give me an answer if a version doesn't exist:
```bash
go get: github.com/opentracing/opentracing-go@fakeversion: invalid version: reading  http://.com 404 Not found
```

#### Finally

This is what I was looking for:

```bash
go get -v -x <package>
```

It will show where it's taken from:
```bash
$ go get -v -x
# get https://myprivateregistry.com/github.com/@v/list
# get https://myprivateregistry.com/github.com/gin-gonic/gin/@v/list
# get https://myprivateregistry.com/github.com/gin-gonic/@v/list
```
