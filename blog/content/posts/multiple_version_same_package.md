---
author: "Nolan"
title: "Using multiple versions of the same library"
date: "2021-09-06"
categories: ["golang", "python"]
draft: false
description: "Golang vs Python"
tags: ["lib", "deps"]
ShowToc: false
TocOpen: false
---

## Golang

Looking into dependency management in golang and I was quite surprised that it's possible to import multiple versions of the same library.

However it only works for major version that are considered as a different module.
Which must be specified when creating the [package](https://github.com/go-redis/redis/blob/v7.0.1/go.mod#L1)

E.g

```golang
module multiversion

go 1.15

require (
	github.com/go-redis/redis/v7 v7.0.0
	github.com/go-redis/redis/v8 v8.0.0
)
```

```golang
package main

import (
	redisv1 "github.com/go-redis/redis/v7"
	redisv2 "github.com/go-redis/redis/v8"
)

func main() {
	redisv1.NewClient(&redisv1.Options{
		Addr:     "localhost:6379",
		Password: "",
		DB:       0,
	})
	redisv2.NewClient(&redisv2.Options{
		Addr:     "localhost:6379",
		Password: "",
		DB:       0,
	})
}
```

## Python




#### Credits:

- https://golangbyexample.com/versiono-module-selection-go
- https://faun.pub/managing-dependency-and-module-versioning-using-go-modules-c7c6da00787a