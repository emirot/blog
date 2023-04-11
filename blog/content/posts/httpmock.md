---
author: "Nolan"
title: "Mocking Http requests in go"
date: "2023-04-10"
categories: ["golang", "http", "mocking"]
draft: false
description: ""
tags: ["golang", "httpmock",]
ShowToc: false
TocOpen: false
---

## Mocking http retries behavior 

As we know, http requests can fail for a large variety of reasons, client side or server side (unauthorized, timeouts etc).  
On the client side a common approach is to retry request by increasing the waiting time between retries.  

After you have decided which approach to use, we will look into how to unit test that.


## Unit Testing 

In golang there is this cool [library](https://github.com/jarcoal/httpmock)

Simple mocking of an http endpoint:

```golang
func TestMockHttpResponse(t *testing.T) {
	httpmock.Activate()
	defer httpmock.DeactivateAndReset()

    httpmock.RegisterResponder("GET", "http://example.com/articles", httpmock.NewStringResponder(200, "my articles").String()))
    resp, err := http.Get("http://example.com/") // return http 200
    ...
}

```

But there is more, we can acutally easily mock retries like this below:

```golang
func TestMockHttpResponse(t *testing.T) {
	httpmock.Activate()
	defer httpmock.DeactivateAndReset()
	first := httpmock.NewStringResponder(500, "")
	second := httpmock.NewStringResponder(200, "good response")

	httpmock.RegisterResponder("GET", "http://myserver.com/workafteronecall", first.Then(second))
    // First call returns 500
    resp, err := http.Get("http://example.com/") // return http 500
    // Second call returns 200

```





