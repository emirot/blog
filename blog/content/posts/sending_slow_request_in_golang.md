---
author: "Nolan"
title: "Sending a slow request"
date: "2022-02-23"
categories: ["golang", "learning", "security"]
draft: false
description: "Sending a slow request in golang"
tags: ["golang", "learning"]
ShowToc: false
TocOpen: false
---

## Sending Slow request in golang

I thought it was a cool snippet that I wanted to share and keep (AKA slow loris attack).  
The http Post method takes an `io.reader` that could be overridden.  
It could be used to test how your web application reacts in those situations, does it hangs? timeouts?

Http Post definition:

`func (c *Client) Post(url, contentType string, body io.Reader) (resp *Response, err error)`

Taking a Read function as example from https://go.dev/src/bytes/buffer.go#L297

```golang
package main

import (
	"fmt"
	"io"
	"net/http"
	"time"
)

func (b *CustomReader) Read(p []byte) (n int, err error) {
	fmt.Println("Read")
	time.Sleep(2000 * time.Millisecond) // slowing down read 
	b.lastRead = opInvalid
	if b.empty() {
		b.Reset()
		if len(p) == 0 {
			return 0, nil
		}
		return 0, io.EOF
	}
	n = copy(p, b.buf[b.off:])
	b.off += n
	if n > 0 {
		b.lastRead = opRead
	}
	return n, nil

}

func makeSlowRequest() {
	start := time.Now()
	customBody := CustomReader{}
	resp, err := http.Post("http://localhost:8080/hello", "text/plain", &customBody)
	if err != nil {
		panic(err)
	}
	fmt.Println(resp)
	fmt.Printf("Duration: %v", time.Since(start))
}

func main() {
	makeSlowRequest()
}
```

