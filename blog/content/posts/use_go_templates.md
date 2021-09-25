---
author: "Nolan"
title: "Go templates"
date: "2021-09-24"
categories: ["golang", "learning"]
draft: false
description: "Templating"
tags: ["golang", "learning"]
ShowToc: false
TocOpen: false
---

## Templating with Sprig

Templating can be useful, for instance it is heavily used in helm.
Below a simple example to get started, sprig is just library on top of golang templating that provides extra functions.

```golang
package main

import (
	"os"
	template "text/template"

	"github.com/Masterminds/sprig"
)

var temp = `
start
{{ .Title | repeat 2 | indent 2}}
{{ .Text }}
{{ first .List }}
end
`

type Values struct {
	Title string
	Text  string
	List  []int
}

func main() {
	t, err := template.New("todos").Funcs(sprig.GenericFuncMap()).Parse(temp)
	if err != nil {
		panic(err)
	}
	s := make([]int, 0)
	s = append(s, 1)
	v := Values{Title: "The title", Text: "Txt", List: s}
	err = t.Execute(os.Stdout, v)
	if err != nil {
		panic(err)
	}
}
```

Full code is there [gitlab](https://gitlab.com/emirot.nolan/golang_tempate/)