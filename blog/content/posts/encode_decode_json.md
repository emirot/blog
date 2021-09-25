---
author: "Nolan"
title: "Golang Encoding/Decoding JSON"
date: "2021-09-25"
categories: ["golang", "learning"]
draft: false
description: ""
tags: ["golang", "learning"]
ShowToc: false
TocOpen: false
---

## Encoding JSON

Encoding Decoding is not as straight forward as other languages as it needs to be mapped to a struct.  
Or a generic map[interface] can be used if you don't know the type in advance.  

```golang
package main

import (
    "encoding/json"
    "fmt"
)

type Data struct {
    Id int `json:"id"`
    List []string `json:"list"`
    NotPrint string `json:"-"`
}


func main(){
    d := Data{Id: 123, List: []string{"abc", "b"},  NotPrint: "This won't print"}
    result, err := json.MarshalIndent(d, "","\t")
    if err != nil {
        panic(err)
    }
    fmt.Printf("%s\n", result)
}
```


## Decoding JSON

```golang
package main

import (
    "encoding/json"
    "fmt"
)

type Data struct {
    Id int `json:"id"`
    List []string `json:"list"`
    NotPrint string `json:"-"`
}


var b = []byte(`
  {
    "id" : 123,
    "list": ["a", "b", "c"]
   }
`)


func main() {
   var d Data
   valid := json.Valid(b)
   if valid {
    json.Unmarshal(b, &d)
    fmt.Printf("%#v", d)
   }

}
```