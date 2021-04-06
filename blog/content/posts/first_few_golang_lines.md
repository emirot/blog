---
author: "Nolan"
title: "First few lines in golang"
date: "2021-03-03T20:35:29-07:00"
categories: ["golang", "learning"]
draft: false
description: "First few lines"
tags: ["golang", "learning"]
ShowToc: false
TocOpen: false
---

# Intro

I've been casually looking into the go language without trying it much.
Now it's time to write so go code :)

# Golang

### Looping

First discovery there is no `while` keyword in golang, I was pretty surprised I thought I was missing something.  
I then look in the [spec](https://golang.org/ref/spec) and there is only `for`.  
I like that they only have limited number of keywords.  

```golang
i := 0
for i < 10 {
    fmt.Println(i)
}
```

### Return statements

Return statement can be specified in function definition  
E.g

```golang
func returnByDefault(s string) (first string, second string) {
	first = "test"
	return
}
```

So even if it looks like nothing is returned, it will return first if declared in the function.  
If we run the function above it will return "test".  


### Init

I thought `main` was the first entry point in golang, looks like if a function is called init it will be called before.  

```golang
func init() {
	fmt.Println("Executed before main")

```

### Datatypes

Not sure if I will get used to declaring types after variable name, well will see.  

```golang
func declaringPrimitivesDatatypes() {
	var s string
	var b bool
	var i uint16
	s = "test"
	b = true
	i = 1
	fmt.Println(s, b, i)
}
```

I prefer the short assigment variable declaration:

```golang
	c, python, java := true, false, "no!"
	fmt.Println(c, python, java)
```

That's it for today, more to come very soon.