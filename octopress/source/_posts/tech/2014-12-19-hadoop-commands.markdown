---
layout: post_tech
title: "Hadoop Commands"
date: 2014-12-19 15:21:49 +0800
comments: true
categories: [tech]
tags: [hadoop, cluster]
toc: true
---

## 1. Operations

All the commands are under `$HADOOP_INSTALL/hadoop/bin`, which are containing `Hadoop DFS` and `Hadoop MapReduce` etc.

```bash
$ start-all.sh      # enabled hadoop daemons: namenode, datanodes, jobtracker, tasktrackers
$ stop-all.sh       # stop all hadoop daemons

$ start-mapred.sh   # start Hadoop Map/Reduce daemons, jobtracker and tasktrackers
$ stop-mapred.sh    # stop Hadoop Map/Reduce daemons

$ start-dfs.sh      # start Hadoop DFS daemons, the namenode and datanodes
$ stop-dfs.sh       # stop Hadoop DFS daemons
```


## 2. Hadoop FS

```bash
$ hadoop fs -cat file:///file3
$ hadoop fs -cat hdfs://host1:port1/file

$ hadoop fs -copyFromLocal <localsrc>URI
$ hadoop fs -copyToLocal <localsrc>URI

$ hadoop fs -get <locaodst>

$ hadoop fs -cp /user/file /uesr/files
$ hadoop fs -cp /user/file1 /user/files /user/dir

$ hadoop fs -du /user/dir1
$ hadoop fs -du hdfs://host:port/user/file
$ hadoop fs -dus <ars>

# clear all in cache
$ hadoop fs -expunge

$ hadoop fs -ls <arg>
$ hadoop fs -lsr
$ hadoop fs -mkdir<path>

$ hadoop fs -mv URI <dest>
$ hadoop fs -rm URI
$ hadoop fs -rmr URI

$ hadoop fs -put <localsrc> <dst>

$ hadoop fs -setrep [R] <path>
$ hadoop fs -test -[ezd] URI
$ hadoop fs -text <src>
```

## 3. HDFS

```bash
# create
$ hdfs dfs -mkdir -p /user/hduser/input

# copy
$ hdfs dfs -put /tmp/gutenberg /user/hduser/input 

# remove
$ hdfs dfs -rm -r /user/hduser/gutenberg/output
```
