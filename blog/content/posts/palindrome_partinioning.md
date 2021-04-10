---
author: "Nolan"
title: "Palindrome Partitioning"
date: "2021-04-01"
categories: ["alogorithm", "python"]
draft: false
description: "Backtracking Algo"
tags: ["algo", "backtracking"]
ShowToc: false
TocOpen: false
---

## Challenge

Given a String `S` return all palindrome partitions.

Example 1:

```
Input: s = "abc"
Output: [["a","b","c"]
```

Example 2:

```
Input: s = "abb"
Output: [["a","bb"],["a", "b", "b"]]
```

## Solution

1. Get all subset of the string.
2. Check if this subset is only composed of palindromes
3. If so add it to the result

```python
class Solution:
    
    def check_palindrome(self, s):
        stack = []
        for e in list(s):
            stack.insert(0, e)
        for e in s:
            i = stack.pop(0)
            if i != e:
                return False
        return True
    
    def get_all_subsets(self, start, s, arr):
        if arr and not 0 in arr:
            self._set.add(tuple(arr[:]))
        for i in range(start, len(s)):
            arr.append(i)
            self.get_all_subsets(i+1, s, arr)
            arr.pop()
        return
    
    def cutter(self, s, cuts):
        res = []
        for cut in cuts:
            cut = list(cut)
            cut = [0] + cut + [len(s)]
            tmp = []
            for i, e in enumerate(cut):
                if i > 0:
                    tmp.append(s[cut[i-1]:cut[i]])
            ct = 0
            for t in tmp:
                if self.check_palindrome(t):
                    ct +=1
            if ct == len(tmp):
                res.append(tmp)
        if self.check_palindrome(s):
            res.append([s])
        return res
    
    def partition(self, s: str) -> List[List[str]]:
        if not s:
            return
        if len(s) == 1:
            return [s]
        self._set = set()
        self.get_all_subsets(0, s, [])
        return self.cutter(s, self._set)
```

| Time complexity | Space Complexity |
|-----------------|----------------- |
| O(2^n)          | O(n)             |