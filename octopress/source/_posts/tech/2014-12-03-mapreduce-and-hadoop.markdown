---
layout: post_tech
title: "MapReduce and Hadoop"
date: 2014-12-03 12:53:01 +0800
comments: true
categories: [tech]
tags: [mapreduce, hadoop, cluster]
toc: false
---

Quoted from Cloudera Community:

MapReduce in Hadoop is extrememly scalable, but was designed for batch processing. 
Even a trivial MapReduce job operating on 10MB of data will take perhaps 20 seconds
 to complete. Adding more nodes to the cluster allows you to store and process more
 data (i.e. it increases capacity and throughput), but it won't make a small job run
 any faster (i.e. it does not decrease latency).

I realized this is an old thread, but I'll point out that Cloudera (disclaimer: my 
employer) released the open source Impala project in the meantime. It has a custom 
execution engine specifically optimized for the interactive queries. This avoids 
the latency I described earlier, so a query that take 20 seconds in MapReduce might 
take 200 milliseconds with Impala.

It's hard to give specific advice to a general question like "at what data size does Hadoop
 begin to shine over traditional DB technologies" because the answer depends on a lot of details.

I would suggest that while data size is an important consideration, it should not be the only
 consideration. The need for scalability to accommodate future data growth is also important, 
as is having a lot of flexibility in how you can process that data. Hadoop allows you to 
process your data with SQL much like a relational database (via Hive and Impala), but also 
through custom MapReduce code, Pig, Mahout, Cascading, Crunch and so on.

IMHO, if you need random access and transaction processing for relatively small accounts 
of highly-structured data, a relational database is the right choice. If you need 
to store large (or growing) amounts of drverse data for analysis using a variety of 
tools, Hadoop is probably the way to go.

It's not really an either-or choice, though. An e-commerce site might use an RDBMS to 
store data about the products people buy, export this data to Hadoop using Sqoop, use 
it to create product recommendations with Mahout, and then export those recommendations 
back to the RDBMS so the web site can display them to customers. It's really about using 
the right tool for the job.



