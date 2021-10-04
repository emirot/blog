---
author: "Nolan"
title: "Go testing"
date: "2021-10-02"
categories: ["golang", "testing"]
draft: false
description: ""
tags: ["golang", "testing"]
ShowToc: false
TocOpen: false
---

## Testing in golang

Simple test in go

```golang
package main

import "testing"

func add(a, b int) int {
	return a + b
}

func TestAddingNumber(t *testing.T) {
	res := add(1, 7)
	if res != 8 {
		t.Errorf("Sum is incorrect, got: %d, expected: %d", res, 8)
	}
}
```

Run tests:

`go test -v ./...`


## Avoiding repeating tests

Instead of duplicating code for simliar tests, there is a different way to test.  

```golang
package main

import (
	"strings"
	"testing"
)

func rep(s string) string {
	return strings.Replace(s, "b", "c", -1)
}
func TestFiles(t *testing.T) {
	tests := []struct {
		testName     string
		oldConfigVal string
		newConfigVal string
		err          error
	}{
		{"Testing First config",
			`a:b`,
			`a:c`,
			nil},
		{"Tesing second config",
			`e:f`,
			`e:f`,
			nil},
	}
	t.Run("Test config replaced correctly", func(t *testing.T) {
		for _, tc := range tests {
			t.Run(tc.testName, func(t *testing.T) {
				res := rep(tc.oldConfigVal)
				t.Log(res, tc.newConfigVal)
				if res != tc.newConfigVal {
					t.Errorf("unable to read file")
				}
			})
		}
	})
}
```