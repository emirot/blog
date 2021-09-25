---
author: "Nolan"
title: "Limit number of concurrent requests"
date: "2021-08-29"
categories: ["golang", "learning"]
draft: true
description: "http request concurrency"
tags: ["golang", "learning", "requests"]
ShowToc: false
TocOpen: false
---

## Limiting the number of concurrent http requests

I was looking to use gorountine in the context of http requests.  
More specificaly, how to limit the number of concurrent of http call made to a service.  
Let's say you are making call to an API that is rate limited.  