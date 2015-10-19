---
layout: post_tech
title: "Apache Hadoop Cluster Installation Guide"
date: 2014-12-04 22:22:00 +0800
comments: true
categories: [tech]
tags: [hadoop, cluster, hive, sqoop, hbase, zookeeper]
toc: true
---

## 1. Cluster

- Stand Alone
- A Single Node
- Cluster

## 2. Components

- Hadoop
- Hive
- Zookeeper
- HBase
- Sqoop


## 3. Guides

Installation steps: hadoop -> hive -> zookeeper -> hbase -> sqoop

NOTES:

- Please follow the steps as listed above, as the latter is relying on the former.
- When start the hadoop services, please also follow the steps as listed above.
- When remove the hadoop services, please follow the reverse steps as listed above.
- All the service commands should be `start` and `stop` right, if `start` but forget `stop` when shut down the machine, 
the HDFS file system would be flagged some errors (to fix the issue, it must format the HDFS system, restart the services 
again, but all the data in the HDFS system would be deleted.)


## 4. Services


| Component	| Type            	        | Port	| Web Interface           |
|:--------------|:------------------------------|:------|:------------------------|
| Hadoop	| NameNode (HDFS)	        | 50070	| http://localhost:50070/ |
|               | JobTracker (MapReduce)	| 50030	| http://localhost:50030/ |
|               | TaskTracker (MapReduce)	| 50060	| http://localhost:50060/ |
|               | cluster (MapReduce with Yarn)	| 8088	| http://localhost:8088/  |
| Hive		|	                        |       |                         |
| Zookeeper	| ClientPort	                | 2181	|                         |
| HBase	        | HMaster    	                | 60010	| http://localhost:60010/ |
|               | HRegionServer	                | 60030	| http://localhost:60030/ |
| Sqoop         |                               |       |                         |


