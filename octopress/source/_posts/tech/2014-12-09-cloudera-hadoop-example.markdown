---
layout: post_tech
title: "Cloudera Hadoop Example"
date: 2014-12-09 20:21:37 +0800
comments: true
categories: [tech]
tags: [cloudera, hadoop, cluster, java]
toc: false
---

Here is the WordCount Example.

## 1. HDFS

```bash
$ sudo su hdfs
$ hadoop fs -mkdir /user/edx
$ hadoop fs -chown edx /user/edx
$ exit
$ sudo su edx
$ hadoop fs -mkdir /user/edx/wordcount /user/edx/wordcount/input
```

## 2. Data

```bash
$ cd
$ mkdir hadoop
$ mkdir hadoop/data
$ cd hadoop/data

$ echo "Hello World Bye World" > file0
$ echo "Hello Hadoop Goodbye Hadoop" > file1
$ hadoop fs -put file* /user/edx/wordcount/input
```

## 3. Programming

```bash
$ cd ~/hadoop
$ mkdir src
$ sudo nano WordCount.java
```

WordCount.java

```java WordCount.java
package org.myorg;

import java.io.IOException;
import java.util.*;

import org.apache.hadoop.fs.Path;
import org.apache.hadoop.conf.*;
import org.apache.hadoop.io.*;
import org.apache.hadoop.mapred.*;
import org.apache.hadoop.util.*;

public class WordCount {

  public static class Map extends MapReduceBase implements Mapper<LongWritable, Text, Text, IntWritable> {
    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(LongWritable key, Text value, OutputCollector<Text, IntWritable> output, Reporter reporter) throws IOException {
      String line = value.toString();
      StringTokenizer tokenizer = new StringTokenizer(line);
      while (tokenizer.hasMoreTokens()) {
        word.set(tokenizer.nextToken());
        output.collect(word, one);
      }
    }
  }

  public static class Reduce extends MapReduceBase implements Reducer<Text, IntWritable, Text, IntWritable> {
    public void reduce(Text key, Iterator<IntWritable> values, OutputCollector<Text, IntWritable> output, Reporter reporter) throws IOException {
      int sum = 0;
      while (values.hasNext()) {
        sum += values.next().get();
      }
      output.collect(key, new IntWritable(sum));
    }
  }

  public static void main(String[] args) throws Exception {
    JobConf conf = new JobConf(WordCount.class);
    conf.setJobName("wordcount");

    conf.setOutputKeyClass(Text.class);
    conf.setOutputValueClass(IntWritable.class);

    conf.setMapperClass(Map.class);
    conf.setCombinerClass(Reduce.class);
    conf.setReducerClass(Reduce.class);

    conf.setInputFormat(TextInputFormat.class);
    conf.setOutputFormat(TextOutputFormat.class);

    FileInputFormat.setInputPaths(conf, new Path(args[0]));
    FileOutputFormat.setOutputPath(conf, new Path(args[1]));

    JobClient.runJob(conf);
  }
}
```

compile

```bash
$ #javac -cp <classpath> -d wordcount_classes WordCount.java
$ javac -cp /opt/cloudera/parcels/CDH/lib/hadoop/*:/opt/cloudera/parcels/CDH/lib/hadoop/client-0.20/* -d /hadoop/src WordCount.java
```

create jar

```bash
$ jar -cvf wordcount.jar -C ~/hadoop/src/ .
```


## 4. Testing


```bash
$ hadoop jar wordcount.jar org.myorg.WordCount /user/edx/wordcount/input /user/edx/wordcount/output

# check the output
$ hadoop fs -cat /user/edx/wordcount/output/part-00000
```

if run, please remove the previous output

```bash
$ hadoop fs -rm -r /user/edx/wordcount/output
```

## Reference

- [Hadoop-Tutorial:ht-usage](http://www.cloudera.com/content/cloudera/en/documentation/HadoopTutorial/CDH4/Hadoop-Tutorial/ht_usage.html)
