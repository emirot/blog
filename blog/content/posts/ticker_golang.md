---
author: "Nolan"
title: "Tickers"
date: "2021-08-30"
categories: ["golang", "learning"]
draft: false
description: "Tickers"
tags: ["golang", "learning"]
ShowToc: false
TocOpen: false
---


## Tickers

I was looking into timers and I came across [tickers](https://golang.org/src/time/tick.go).  
Basically they allow you to trigger an event at a regular time interval.  
One simple example could be some kind of monitoring.  

What's interesting is that it pushes a Ticker to a channel.  

Here is a basic example:

```golang
package main

import (
	"fmt"
	"os"
	"sync"
	"time"
)

func checkInBackground(ticker time.Ticker, wg *sync.WaitGroup) {
    wg.Add(1)
	for ; true; <-ticker.C {
		fmt.Println("ok")
	}
	wg.Done()
}

func termination() {
	os.Exit(1)
}

func main() {
	var wg sync.WaitGroup
	ticker := time.NewTicker(1 * time.Second)
	defer ticker.Stop()
	go checkInBackground(*ticker, &wg)
	time.AfterFunc(time.Duration(10)*time.Second, termination)
	wg.Wait()
	fmt.Println("Done as expected")
}
```