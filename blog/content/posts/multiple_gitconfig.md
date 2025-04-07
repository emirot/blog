---
author: "Nolan"
title: "Multiple gitconfigs"
date: "2025-03-05"
categories: ["git"]
draft: false
description: "git"
tags: ["git", "config"]
ShowToc: false
TocOpen: false
---

## Multiple gitconfig

Having multiple git user some at work, personal can create some pain, wrong user pushing to a repository etc.
There is a better way and it's using gitconfig per path, it is very simple and very useful.

E.g of `~/.gitconfig`


```
[includeIf "gitdir:/Users/<username>/Documents/personal/**"]
    path = /Users/<username>/Documents/.gitconfig-personal
[includeIf "gitdir:/Users/<username>/Documents/repositories/**"]
    path = /Users/<username>/Documents/.gitconfig-<work>

[core]
	excludesfile = /Users/<user>/.gitignore

```

In each of the directories you will need to create another gitconfig with the user it belongs to.

E.g of `/Users/<username>/Documents/.gitconfig-personal`

```
[user]
	name = firstname lastname
	email = myemail@gmail.com

```