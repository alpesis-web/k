---
layout: post_tech
title: "HBase Commands"
date: 2014-12-18 14:39:10 +0800
comments: true
categories: [tech]
tags: [hbase, cluster]
toc: true
---

HBase commands

- operations
- table
- record

## 1. open and close

```bash
$ ./bin/hbase shell
hbase(main):001:0>

hbase> exit  # To exit the HBase Shell and disconnect from your cluster
hbase> quit  # HBase is still running in the background
```

## 2. table

```bash
hbase> create 'test', 'cf'    
0 row(s) in 1.2200 seconds

hbase> list 'test'
TABLE
test
1 row(s) in 0.0350 seconds

=> ["test"]

hbase> disable 'test'
0 row(s) in 1.6270 seconds

hbase> enable 'test'
0 row(s) in 0.4500 seconds

hbase> drop 'test'
0 row(s) in 0.2900 seconds   
```

## 3. data

```bash
hbase> put 'test', 'row1', 'cf:a', 'value1'
0 row(s) in 0.1770 seconds

hbase> put 'test', 'row2', 'cf:b', 'value2'
0 row(s) in 0.0160 seconds

hbase> put 'test', 'row3', 'cf:c', 'value3'
0 row(s) in 0.0260 seconds 

hbase> scan 'test'
ROW                   COLUMN+CELL
 row1                 column=cf:a, timestamp=1403759475114, value=value1
 row2                 column=cf:b, timestamp=1403759492807, value=value2
 row3                 column=cf:c, timestamp=1403759503155, value=value3
3 row(s) in 0.0440 seconds

hbase> get 'test', 'row1'
COLUMN                CELL
 cf:a                 timestamp=1403759475114, value=value1
1 row(s) in 0.0230 seconds
```

