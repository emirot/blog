---
author: "Nolan"
title: "Using Variadic in Go"
date: "2021-10-03"
categories: ["golang", "variadic"]
draft: false
description: "Getting dependencies via goproxy"
tags: ["golang", "goproxy",]
ShowToc: false
TocOpen: false
---

## Nothing new

Variadic arguments are present in many many languages.  

In golang the syntax is like so:

```golang
package main

import "fmt"

func addNubers(nums ...int) int {
	s := 0
	for _, n := range nums {
		s += n
	}
	return s
}

func main() {
	res := addNubers(1,2,3)
}
```

In python:

```python
def func(a, *t):
	for e in t:
		print(e)

func(1,2,3,3,4)
```


## Usage in go

In go variadic functions are used to make configuration easily extendable, without changing existing interface.  
By doing so we can easily add options without breaking existing code.  

```golang
package main

import "fmt"

type MyServer struct {
	addr   string
	tls    bool
	port   string
	logger string
}

type Option func(s *MyServer)

func WithAddr(addr string) Option {
	return func(s *MyServer) {
		s.addr = addr
	}
}

func withPort(port string) Option {
	return func(s *MyServer) {
		s.port = port
	}
}

func NewMyServer(opts ...Option) *MyServer {
	server := MyServer{}
	for _, f := range opts {
		f(&server)
	}
	return &server
}

func main() {
	fmt.Println("start")
	s := NewMyServer(withPort("1234"), WithAddr("nolanemirot.com"))
	fmt.Println(s.port, s.addr)
}

```