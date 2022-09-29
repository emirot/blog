---
author: "Nolan"
title: "Monkey Patching in Golang"
date: "2022-09-27"
categories: ["golang", "testing"]
draft: false
description: "Testing"
tags: ["test","patch", "golang"]
ShowToc: false
TocOpen: false
---

## Monkey Patching

As I've seen monkey patching is not advise in golang but it is possible here is a simple example.

Code to test:

```golang
package main

var version = func(test string) string {
	return "true version" + test
}

func getVersion() string {
	return version("test")
}
```

Test

```golang
package main

import (
	"testing"
)

func TestVersion(t *testing.T) {
	oldfunction := version
	defer func() { version = oldfunction }()
	version = func(name string) string {
		return "fake return output"
	}
	if getVersion() != "fake return output" {
		t.Fatalf("failed" + getVersion())
	}
}
```