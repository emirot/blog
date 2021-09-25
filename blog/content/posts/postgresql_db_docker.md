---
author: "Nolan"
title: "Postgresql in Docker"
date: "2021-09-21"
categories: ["docker", "posgresql"]
draft: false
description: "Up and running DB in seconds"
tags: ["postgresql", "docker"]
ShowToc: false
TocOpen: false
---

## Running DB from a dump in seconds

I will take this [free dataset](https://www.postgresql.org/ftp/projects/pgFoundry/dbsamples/french-towns-communes-francais/french-towns-communes-francaises-1.0/) from postgresql website.  

```bash
docker run  -d --name my_db -v /Users/nolan/Downloads/french-towns-communes-francaises-1.0/french-towns-communes-francaises.sql:/docker-entrypoint-initdb.d/create_table.sql  -p 54320:5432 -e POSTGRES_PASSWORD=my_password  -e POSTGRES_USER=nolan postgres
```

## Connect to DB

```bash
 pgcli -h localhost -p54320 -Unolan
```
nolan@localhost:nolan> SELECT count(*) FROM towns where name like 'Mont%';
+---------+
| count   |
|---------|
| 826     |
+---------+
SELECT 1
Time: 0.014s
```
