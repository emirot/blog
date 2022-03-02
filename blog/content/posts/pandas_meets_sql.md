---
author: "Nolan"
title: "Pandas meets SQL"
date: "2021-03-03"
categories: ["python", "pysql"]
draft: false
description: "Exploring data with SQL (pysql)"
tags: ["data", "pandas"]
ShowToc: false
TocOpen: false
---

## Pandas meets SQL

I was exploring data from a CSV, I used Jupyter notebook as I'm a bit familiar with it and it's a great fit for this kind of use case.  
It's very easy to load data in a dataframe, however I don't use pandas often enough and cannot remember all the functions.  
I just wanted to write SQL queries from the dataframe itself.

```python
import matplotlib.pyplot
import pandas as pd
from pandasql import sqldf

df = pd.read_csv('data.csv')


pysqldf = lambda q: sqldf(q, globals())
q = """SELECT
       SUBSTR(deployment_date, 1, 9) as date, SUBSTR(deployment_date, 1, 9) - SUBSTR(created_date, 1, 9)  as ct
       FROM df 
       where deploy_type='k8s' and SUBSTR(created_date, 1, 9) > '12/27/2021'
       """
df = pysqldf(q)
```

It's a cool project, however I would not using it much for 2 reasons:
- Not maintained last commit was more than 4 years ago
- This is SQLite syntax which is sometimes limiting
