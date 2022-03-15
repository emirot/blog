---
author: "Nolan"
title: "Adding service DB in Github action"
date: "2022-03-05"
categories: ["github"]
draft: false
description: ""
tags: ["CICD"]
ShowToc: false
TocOpen: false
---

This is so useful for testing purposes, plus it's getting easier to do so.  
Let's take a look for PostgreSQL and MySQL.  

### Adding PostgreSQL in Github action

```yml
name: Build
on: push
jobs:
  build:
    runs-on: ubuntu-latest
    services:
    # Label used to access the service container
      postgres:
        # Docker Hub image
        image: postgres:13
        # Provide the password for postgres
        env:
          POSTGRES_USER: root
          POSTGRES_PASSWORD: test
          POSTGRES_DB: sqljson
        # Set health checks to wait until postgres has started
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
         - 5432:5432
    steps:
      - uses: actions/checkout@v2
      - name: Setup go
        uses: actions/setup-go@v2
        with:
          go-version: ^1.17
      - name: Install go migrate
        run: | 
          curl -L https://github.com/golang-migrate/migrate/releases/download/v4.15.1/migrate.linux-amd64.tar.gz | tar xvz;
          sudo mv migrate /usr/bin/migrate
      - name: Run migration
        run: migrate -path db/migration -database "postgresql://root:test@localhost:5432/sqljson?sslmode=disable" -verbose up
      - run: go test -v ./...
```


### Adding MySQL in Github action

I wanted to do the exact same for MySQL but it's even simpler because mysql is already present, you just need to start it.


```yaml
name: CI
on: push
jobs:
  mysqltest:
    runs-on: ubuntu-latest
    strategy:
     matrix:
      go: [ '1.16', '1.17' ]
    name: Go ${{ matrix.go }} sample
    env:
      DB_DATABASE: sqljson
      DB_USER: root
      DB_PASSWORD: 'root'
      DB_HOST: localhost
    steps:
      - run: |
          sudo /etc/init.d/mysql start
          mysql -e 'CREATE DATABASE sqljson;' -uroot -proot
          mysql -e 'SHOW DATABASES;' -uroot -proot
      - uses: actions/checkout@v2
      - name: Setup go
        uses: actions/setup-go@v2
        with:
          go-version: ${{ matrix.go }}
      - name: Install go migrate
        run: | 
          curl -L https://github.com/golang-migrate/migrate/releases/download/v4.15.1/migrate.linux-amd64.tar.gz | tar xvz;
          sudo mv migrate /usr/bin/migrate
      - name: Run migration
        run: migrate -path db/migration -database "mysql://root:root@tcp(localhost:3306)/sqljson" -verbose up
      - run: go test -timeout 30s -run ^TestMysqlTable$ github.com/emirot/sqlgojson/src/sqlgojson
```
