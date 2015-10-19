---
layout: post_tech
title: "Hadoop MapReduce Example"
date: 2014-12-05 14:55:17 +0800
comments: true
categories: [tech]
tags: [hadoop, mapreduce, cluster]
toc: true
---

Requirements:

- Hadoop
- MapReduce
- Yarn
- Gutenberg

## 1. Data

download books

```bash
$ cd /tmp
$ mkdir gutenberg

$ wget http://www.gutenberg.org/cache/epub/4300/pg4300.txt
$ wget http://www.gutenberg.org/cache/epub/5000/pg5000.txt
$ wget http://www.gutenberg.org/cache/epub/20417/pg20417.txt
```

## 2. HDFS

copy data to HDFS

```bash
# hadoop 1.0.x
hadoop dfs -copyFromLocal /tmp/gutenberg /user/hduser/gutenberg

# hadoop 2.0.x
hdfs dfs -mkdir -p /user/hduser/input
hdfs dfs -mkdir -p /user/hduser/output
hdfs dfs -put /tmp/gutenberg /user/hduser/input 

# check
hadoop dfs -ls /user/hduser
hadoop dfs -ls /user/hduser/gutenberg
```

## 3. MapReduce

- requirement: hadoop-mapreduce-examples-2.6.0.jar

```bash
$ hdfs dfs -rm -r /user/hduser/gutenberg/output

$ cd /usr/local/hadoop/share/hadoop/mapreduce/
$ hadoop jar hadoop-mapreduce-examples-2.6.0.jar wordcount /user/hduser/gutenberg/input/gutenberg /user/hduser/gutenberg/output
$ hadoop jar hadoop-mapreduce-examples-2.6.0.jar wordcount -D mapred.reduce.tasks=16 /user/hduser/gutenberg/input/gutenberg /user/hduser/gutenberg/output
```

NOTE: `/user/hduser/gutenberg/output`, it must be deleted for each run.

outputs:

```bash
  aggregatewordcount: An Aggregate based map/reduce program that counts the words in the input files.
  aggregatewordhist: An Aggregate based map/reduce program that computes the histogram of the words in the input files.
  bbp: A map/reduce program that uses Bailey-Borwein-Plouffe to compute exact digits of Pi.
  dbcount: An example job that count the pageview counts from a database.
  distbbp: A map/reduce program that uses a BBP-type formula to compute exact bits of Pi.
  grep: A map/reduce program that counts the matches of a regex in the input.
  join: A job that effects a join over sorted, equally partitioned datasets
  multifilewc: A job that counts words from several files.
  pentomino: A map/reduce tile laying program to find solutions to pentomino problems.
  pi: A map/reduce program that estimates Pi using a quasi-Monte Carlo method.
  randomtextwriter: A map/reduce program that writes 10GB of random textual data per node.
  randomwriter: A map/reduce program that writes 10GB of random data per node.
  secondarysort: An example defining a secondary sort to the reduce.
  sort: A map/reduce program that sorts the data written by the random writer.
  sudoku: A sudoku solver.
  teragen: Generate data for the terasort
  terasort: Run the terasort
  teravalidate: Checking results of terasort
  wordcount: A map/reduce program that counts the words in the input files.
  wordmean: A map/reduce program that counts the average length of the words in the input files.
  wordmedian: A map/reduce program that counts the median length of the words in the input files.
  wordstandarddeviation: A map/reduce program that counts the standard deviation of the length of the words in the input files.
```

check the result:

```bash
$ hadoop dfs -ls /user/hduser/gutenberg/output
$ hadoop dfs -cat /user/hduser/gutenberg/output/part-r-00000
```

copy to local

```bash
$ hadoop dfs -getmerge /user/hduser/gutenberg/output/ /tmp/gutenberg/output
$ head /tmp/gutenberg/output
```


## 4. UI

- http://localhost:50070/
- http://localhost:8088/


