---
author: "Nolan"
title: "Golang code coverage badge in gitlab"
date: "2021-04-16"
categories: ["golang", "gitlab", "codecoverage"]
draft: false
description: "Broken link checker"
tags: ["golang", "gitlab", "codecoverage"]
ShowToc: false
TocOpen: false
---

## Adding Golang code coverage badge in gitlab

Currently there is no example in gitlab [official documentation](https://docs.gitlab.com/ee/user/project/merge_requests/test_coverage_visualization.html) regarding code coverage for golang.

However I found:

- https://medium.com/@ulm0_/golang-multi-packages-test-coverage-with-gitlab-ci-a7b52b91ef34
- https://github.com/t-yuki/gocover-cobertura

So I tried them out and it looks to work fine.

## Live Example

I quickly made a test repo: [https://gitlab.com/emirot.nolan/go-test-coverage/](https://gitlab.com/emirot.nolan/go-test-coverage/)
To try that out and it work fine !

Here is the gitlab-ci.yml

```yaml
stages:
    - coveragetest

test:
    stage: coveragetest
    image: golang
    script:
        - go get github.com/t-yuki/gocover-cobertura
        - go test  ./... -v -coverprofile coverage.txt
        - gocover-cobertura < coverage.txt > coverage.xml
    coverage: /^coverage:\s(\d+(?:\.\d+)?%)/
    artifacts:
      name: $CI_JOB_NAME/coverage.txt
      paths:
        - coverage.xml
      expire_in: 2 days
      reports:
        cobertura: coverage.xml
```

Then in order to get the coverage badge it can be added in the readme or in the repo settings>general>badges.

Or in the readme, for my repo like so:

```
[![coverage report](https://gitlab.com/emirot.nolan/go-test-coverage/badges/master/coverage.svg)](https://gitlab.com/emirot.nolan/go-test-coverage/-/commits/master)
```