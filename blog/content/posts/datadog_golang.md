---
author: "Nolan"
title: "Monitoring golang web app with Datadog"
date: "2021-04-15"
categories: ["golang", "monitoring"]
draft: false
description: "Broken link checker"
tags: ["golang", "datadog"]
ShowToc: false
TocOpen: false
---

## Monitoring golang web application with Datadog

Adding datdog monitoring for java was a matter of minutes, so I thought for golang it would be even faster.

Get the datadog jar add it the the dockerfile entrypoint and that's it. (assuming datadog env variables are set up correctly in k8s)

It will be something like:

```Dockerfile
RUN curl --retry 3 --connect-timeout 3 -Ss -o dd-java-agent.jar https://github.com/DataDog/dd-trace-java/releases/download/v0.78.1/dd-java-agent.jar
ENTRYPOINT [ "java", \
    "-javaagent:dd-java-agent.jar", \
    "-jar", "app.jar", ]
```
And it worked, let's look into it for golang.


## Istio proxy start time

This issue is very well explained here: https://www.openshift.com/blog/how-to-make-istio-work-with-your-apps  
It can be sum up to: ` The application container in a pod tries to make initial network connections at start time, but it fails to reach the network.`

In the log it will show: 
```
Datadog Tracer v1.27.0 WARN: DIAGNOSTICS Unable to reach agent: Post \"http://10.42.147.44:8126/v0.4/traces\": dial tcp 10.42.147.44:8126: connect: connection refused
```

You would assume that it's only at startup time and Datadog will retry, well the answer is **NO**.  
Well there is many way to fix this issue as explained in the openshift post.

## Deprecated usage

If by any change you use that syntax:
```golang
tracer.Start(
		tracer.WithServiceName(svc),
		)
```

You won't see it in datadog console ... 

After looking in the log you will get this nice error:
```text
Datadog Tracer v1.29.1 WARN: ddtrace/tracer: deprecated config WithServiceName should not be used with `WithService` or `DD_SERVICE`; integration service name will not be set.
```

Ok removing that `WithServiceName` and now I can see it in datadog console.

## Contrib

Great! I finally see my app in the datadog console, but where are my endpoints? Can't see them.
The reason is that a middleware needs to be added if you are using net/http for example:


```golang
package main

import (
	"fmt"
	"log"
	"net/http"
	"os"

	httptrace "gopkg.in/DataDog/dd-trace-go.v1/contrib/net/http"
	"gopkg.in/DataDog/dd-trace-go.v1/ddtrace/tracer"
)

func health(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Health OK!")
}

func nolan(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "nolan!")
}


func main() {
	tracer.Start()
	defer tracer.Stop()
	mux := httptrace.NewServeMux()
	mux.HandleFunc("/", nolan)
	mux.HandleFunc("/health", health)
	log.Fatal(http.ListenAndServe(":80", mux))

```
That's all for today.