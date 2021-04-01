---
author: "Nolan"
title: "Back to blogging"
date: "2021-03-31T20:35:29-07:00"
categories: ["blog", "hugo"]
draft: false
description: "First blog post on my new blog!"
tags: ["hugo", "blog"]
ShowToc: false
TocOpen: false
---


# Hugo Blogging

I've tried different blog platform over the years: ghost, wordpress.

Hugo is very popular nowadays, so I wanted to try it out.

Moreover, I came accross that theme that looked cool: [https://themes.gohugo.io/hugo-papermod/](https://themes.gohugo.io/hugo-papermod/)

# AWS Amplify

I also wanted to try out AWS Amplify.
I'm becoming very familiar with gitlab and `gitlab-ci.yml` and wanted to discover what AWS had to offer.

After playing a bit with it, it was quicker than I thought.

```
version: 1.0
env:
  variables:
    HUGOVERSION: v0.82.0
    HUGOINSTALL: hugo_extended_0.82.0_Linux-64bit.tar.gz
frontend:
  phases:
    # IMPORTANT - Please verify your build commands
    preBuild:
      commands:
        - wget https://github.com/gohugoio/hugo/releases/download/$HUGOVERSION/$HUGOINSTALL
        - tar -xf $HUGOINSTALL
        - mv hugo /usr/bin/hugo
        - rm -rf $HUGOINSTALL
        - cd blog
        - git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod --depth=1
        - git submodule update --init --recursive
        - ls -la
        - hugo version
    build:
      commands:
        - hugo -D
        - ls public
  artifacts:
    # IMPORTANT - Please verify your build output directory
    baseDirectory: blog/public
    files:
      - '**/*'
  cache:
    paths: []
```

And voila, it's up and running!  
I then just had to change the `baseURL: "/"` in config.yml in order for the assests to load correctly.
