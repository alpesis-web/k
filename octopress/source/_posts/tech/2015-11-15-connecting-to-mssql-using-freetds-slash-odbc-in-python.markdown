---
layout: post_tech
title: "Connecting to MSSQL Using FreeTDS / ODBC in Python"
date: 2015-11-15 21:23:21 +0800
comments: true
categories: [tech]
tags: [linux, db, debian, python, mssql, freetds, odbc]
toc: true
---

## Prequisitions

- OS: Debian 7.8
- Dependencies: freetds-dev, freetd-bin, unixodbc-dev, tdsodbc
- Python packages: pyodbc, sqlalchemy

## Steps

Installing the dependencies

```bash
$ sudo apt-get install freetds-dev freetds-bin unixodbc-dev tdsodbc
$ pip install pyodbc sqlalchemy
```

Configuring `/etc/odbcinst.ini`

```bash /etc/odbcinst.ini
[FreeTDS]
Description     = TDS driver (Sybase/MS SQL)
Driver          = /usr/lib/x86_64-linux-gnu/odbc/libtdsodbc.so
Setup           = /usr/lib/x86_64-linux-gnu/odbc/libtdsS.so
CPTimeout       =
CPReuse         =
UsageCount      = 1
```

Configuring `/etc/freetds/freetds.conf`

```bash /etc/freetds/freetds.conf
[global]
    port = 1433
    tds version = 7.0
```

Testing

```bash
$ tsql -H host_name -p 3688 -U user_name
```

### example: pyodbc

```python
import pyodbc


def connect_db(HOST, DATABASE, UID, PWD, TDS_Version):

    connection = pyodbc.connect('DRIVER=FreeTDS; \
                             SERVER=HOST; \
                             PORT=1433; \
                             DATABASE=DATABASE; \
                             UID=UID; \
                             PWD=PWD; \
                             TDS_Version=8.0;')

    cursor = connection.cursor()
    return cursor

def query_data(cursor, query):

    for row in cursor.execute(query):
        print row

if __name__ = '__main__':

    HOST = ''                         # server
    DATABASE = ''                     # database_name
    UID = ''                          # user_name
    PWD = ''                          # password
    TDS_Version = ''                  # TDS_Version

    cursor = connect_db(HOST, DATABASE, UID, PWD, TDS_Version)
    query_data(cursor, 'select 6 * 7 as [Result];')

```

### example: sqlalchemy


```python

import urllib
from sqlalchemy import create_engine

def connect_db(HOST, DATABASE, UID, PWD, TDS_Version):

    db_settings = 'DRIVER=FreeTDS; \
                   SERVER=%s; \
                   PORT=1433; \
                   DATABASE=%s; \
                   UID=%s; \
                   PWD=%s; \
                   TDS_Version=%s'.format(HOST, DATABASE, UID, PWD, TDS_Version)
    engine = create_engine('mssql+pyodbc:///?odbc_connect' + urllib.quote_plus(db_settings))

    return engine

def query_db(engine, query):

    for row in engine.excute(query):
        print row

if __name__ = '__main__':

    HOST = ''                         # server
    DATABASE = ''                     # database_name
    UID = ''                          # user_name
    PWD = ''                          # password
    TDS_Version = ''                  # TDS_Version

    engine = connect_db(HOST, DATABASE, UID, PWD, TDS_Version)
    query_data(engine, 'select 6 * 7 as [Result];')
```
