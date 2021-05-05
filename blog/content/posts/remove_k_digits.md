---
author: "Nolan"
title: "Remove K-digits"
date: "2021-05-03"
categories: ["algorithm", "greedy"]
draft: true
description: "Another programing challenge"
tags: ["pyhon", "algorithm"]
ShowToc: false
TocOpen: false
---

## Challenge

Given a non-negative number represented as a string, return the smaller possible integer after removing K digits.
E.g
```
"1041"
1
```
Will output `41`

## Solution 1

I really liked my first solution, however performances can't be worst.
It's a brute force approach doing backtracking, basically I tried to remove 3 digits in every possible position.

```python
from copy import deepcopy
class Solution:
    def helper(self, num, k, arr, start):
        if len(arr) == k:
            self.res.append(deepcopy(arr))
            return 
        for i in range(start, len(num)):
            arr.append(i)
            self.helper(num, k, arr, i+1)
            arr.remove(i)
        return
    
    def remove_from_list(self, num, to_remove):
        num = list(num)
        r = []
        for i, e in enumerate(num):
            if i not in to_remove:
                r.append(e)
        return r
    
    def removeKdigits(self, num: str, k: int) -> str:
        if k == len(num):
            return  "0"
        self.res = []
        self.helper(tuple(num), k, [], 0)
        r = int(num)
        for i in self.res:
            ret = self.remove_from_list(num, i)
            ret =  int("".join(ret))
            r  = min(r, ret)
        return str(r)

```

## Solution 2

After failing on a lot of different test cases it looks like this greedy implement below works.  

I still had to handle some edge cases e.g `1112`, 2 must be remove but in `1041` 1 needs to be removed.

```python
class Solution:

    def remove_zero_in_front(self, string):
        i = 0
        while i < len(string) and string[i] == "0":
            i += 1
        return string[i:]

    def find_biggest_and_remove(self, num):
        m = 0
        for i in (num):
            m = max(m, int(i))
        num = list(num)
        snum = "".join(num)
        pos = snum.find(str(m))
        newstr = num[:pos] + num[pos+1:]
        return "".join(newstr)

    def removeKdigits(self, num: str, k: int) -> str:
        if k >= len(num):
            return "0"
        i = 1
        num = list(num)
        while i < len(num) and k > 0:
            if num[i-1] > num[i]:
                num.pop(i-1)
                i = 0
                k -= 1
            i += 1
        res = "".join(num)
        while k > 0:
            res = self.find_biggest_and_remove(res)
            k-=1
        res = self.remove_zero_in_front(res)
        if res == "":
            return "0"
        return res
```

Time complexity: n*n
Space complexity: n