---
author: "Nolan"
title: "Golang, Adding static content in binaries"
date: "2022-09-09"
categories: ["golang"]
draft: false
description: ""
tags: ["golang"]
ShowToc: false
TocOpen: false
---

## Embeded directive

When building a binary, if you load config files, they won't be added to your binary automatically. (The `embeded` package was added in golang 1.16 before then some external libraries could be used). 
You have to specifically tell the compiler to add them, by using `embeded`.


```golang
import _ "embed"
//go:embed my_config.yaml
var config string
```

From what I've seen, it cannot be done in a function it has to be global.
It can also work on multiple files, by using a wildcard, as seen in the [documentation](https://pkg.go.dev/embed)