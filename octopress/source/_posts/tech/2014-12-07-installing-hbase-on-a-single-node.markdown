---
layout: post_tech
title: "Installing HBase on A Single Node"
date: 2014-12-07 14:29:58 +0800
comments: true
categories: [tech]
tags: [hbase, hadoop, cluster]
toc: true
---

## Requirements

- Ubuntu 12.04
- Hadoop
- Hive
- Zookeeper


HBase Serviecs including:

- HMaster
- HRegionServer

## 1. Installation

```bash
$ sudo wget http://mirrors.oschina.net/apache/hbase/hbase-0.98.3/hbase-0.98.3-hadoop2-bin.tar.gz
$ sudo tar xzf hbase-0.98.3-hadoop2-bin.tar.gz
$ sudo mv hbase-0.98.3-hadoop2 /usr/local/hbase

$ sudo chown -R hduser:hadoop /usr/local/hbase
```


## 2. Configuration

- .bashrc
- hbase-env.sh
- hbase-site.xml
- regionservers

### 2.1. .bashrc

```bash
$ sudo nano $HOME/.bashrc
```

.bashrc

```bash
# Set HBASE_HOME
export HBASE_HOME=/usr/local/hbase
export PATH=$PATH:$HBASE_HOME/bin
```

### 2.2. hbase-env.sh

```bash
$ sudo nano /usr/local/hbase/conf/hbase-env.sh
```

hbase-env.sh

```bash
export JAVA_HOME=/usr/lib/jvm/java-6-openjdk-amd64
```

### 2.3. hbase-site.xml

```bash
sudo nano /usr/local/hbase/conf/hbase-site.xml
```

hbase-site.xml

```xml hbase-site.xml
   <property>
       <name>hbase.rootdir</name>
       # NOTE: localhost must be as the same as regionserver, if regionserver is hdmaster, here is hdmaster
       <value>hdfs://localhost:54310/user/hbase</value>            
   </property>

   <property>
       <name>hbase.cluster.distributed</name>
       <value>true</value>
   </property>

   <property>
       <name>hbase.zookeeper.quorum</name>
       <value>localhost</value>
   </property>

   <property>
       <name>dfs.replication</name>
       <value>1</value>
   </property>

   <property>
       <name>hbase.zookeeper.property.dataDir</name>
       <value>/app/zookeeper</value>
   </property>

   <property>
       <name>hbase.zookeeper.property.clientPort</name>
       <value>2181</value>
   </property>
```

### 2.4. regionservers

```bash
$ sudo nano /usr/local/hbase/conf/regionservers
```

regionservers

```
localhost
```

## 3. Create HDFS folders

```bash
$ sudo mkdir /app/zookeeper
$ sudo chown -R hduser:hadoop /app/zookeeper
$ sudo chmod 755 /app/zookeeper
```

## 4. Testing

```bash
$ start-hbase.sh
$ jps
$ hbase shell
```

outputs

```bash
hduser@precise64:/usr/local/hbase/conf$ start-hbase.sh
starting master, logging to /usr/local/hbase/logs/hbase-hduser-master-precise64.out

hduser@precise64:/usr/local/hbase/conf$ jps
11001 NameNode
11127 DataNode
11535 NodeManager
12097 HMaster
12283 Jps
11435 ResourceManager
11916 QuorumPeerMain
12221 HRegionServer
11286 SecondaryNameNode
```

NOTE: 

- If zookeeper is enabled, `HMaster` and `HRegionServer` must be running.
- If disabled, HBase uses the default zookeeper, then `HMaster`, `HRegionServer` and `HQuorumPeer` must be running.

hbase shell

```bash
hduser@precise64:/usr/local/hbase/conf$ hadoop fs -ls /user/hbase
Found 5 items
drwxr-xr-x   - hduser supergroup          0 2014-12-18 16:31 /user/hbase/.tmp
drwxr-xr-x   - hduser supergroup          0 2014-12-18 16:31 /user/hbase/data
-rw-r--r--   1 hduser supergroup         42 2014-12-18 16:31 /user/hbase/hbase.id
-rw-r--r--   1 hduser supergroup          7 2014-12-18 16:31 /user/hbase/hbase.version
drwxr-xr-x   - hduser supergroup          0 2014-12-18 16:31 /user/hbase/oldWALs
```

## 5. UI

- HMaster: http://localhost:60010
- HRegionServer: http://localhost:60030


## References

- [HBase Quick Start](http://hbase.apache.org/book/quickstart.html)
- [installing-pseudo-distributed-hbase-on-ubuntu](https://archanaschangale.wordpress.com/2013/08/31/installing-pseudo-distributed-hbase-on-ubuntu/)
- [HBase run modes: Standalone and Distributed](http://hbase.apache.org/book/standalone_dist.html)
