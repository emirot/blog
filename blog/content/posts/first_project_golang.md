---
author: "Nolan"
title: "First golang project"
date: "2021-04-09"
categories: ["golang", "learning"]
draft: false
description: "First few lines"
tags: ["golang", "learning", "import"]
ShowToc: false
TocOpen: false
---

## Import Function form package

I thought it was going to be similiar to many languages and I will just have to just do something like:

```golang
import "mypakckage/Myfuction"
```
Well... no, first it will depend on your GOPATH, which is handled differently depending of your go version.
Secondly, In golang exported function must be in Uppercase, otherwise it won't compile!

## First module

So I chose the easiest that I could find first to structure project by creating a module.  

Create a go module:  

```bash
go mod init github.com/emirot/go-guitar
```

It will create a `go.mod` file like so:  
```golang
module github.com/emirot/go-guitar

go 1.16
```

And then I could finally do  

```golang
import "github.com/emirot/go-guitar/api/guitar"
```

This test project can be found here: [https://github.com/emirot/go-guitar](https://github.com/emirot/go-guitar)