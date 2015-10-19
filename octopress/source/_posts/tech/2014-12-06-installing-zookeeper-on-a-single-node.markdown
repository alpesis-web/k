---
layout: post_tech
title: "Installing Zookeeper on A Single Node"
date: 2014-12-06 10:09:02 +0800
comments: true
categories: [tech]
tags: [zookeeper, hadoop, cluster]
toc: true
---

## Prequisitions

- Ubuntu 12.04
- Hadoop
- Hive

## 1. Installation

```bash
$ sudo wget http://mirrors.oschina.net/apache/zookeeper/stable/zookeeper-3.4.6.tar.gz
$ sudo tar xzf zookeeper-3.4.6.tar.gz
$ sudo mv zookeeper-3.4.6 /usr/local/zookeeper

$ cd /usr/local/zookeeper
$ sudo chown -R hduser:hadoop /usr/local/zookeeper
```

## 2. Configuration

### 2.1. .bashrc

```bash
$ sudo nano $HOME/.bashrc
$ source $HOME/.bashrc
```

.bashrc

```bash .bashrc
# Set ZOOKEEPER_HOME
export ZOOKEEPER_HOME=/usr/local/zookeeper
export PATH=$PATH:$ZOOKEEPER_HOME/bin
```

### 2.2. zoo.cfg

```bash
$ cd /usr/local/zookeeper/conf
$ cp zoo_sample.cfg zoo.cfg
$ sudo nano zoo.cfg
```

zoo.cfg

```bash zoo.cfg
tickTime=2000
dataDir=/var/zookeeper
clientPort=2181
```

notes:

- tickTime: the basic time unit in milliseconds used by ZooKeeper. It is used to do heartbeats and the minimum session timeout will be twice the tickTime.
- dataDir: the location to store the in-memory database snapshots and, unless specified otherwise, the transaction log of updates to the database.
- clientPort: the port to listen for client connections


## 3. Testing

```bash
$ zkServer.sh start
$ jps
$ zkCli.sh 127.0.0.1:2181
```

example

```bash
hduser@precise64:/usr/local/zookeeper/conf$ zkServer.sh start
JMX enabled by default
Using config: /usr/local/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED

hduser@precise64:/usr/local/hbase/conf$ jps
6176 Jps
3347 QuorumPeerMain
```

NOTE: If `QuorumPeerMain`, then succeed.

connect zookeeper

```bash
hduser@precise64:/usr/local/zookeeper/conf$ zkCli.sh 127.0.0.1:2181
Connecting to localhost:2181
2014-12-16 09:07:45,083 [myid:] - INFO  [main:Environment@100] - Client environment:zookeeper.version=3.4.6-1569965, built on 02/20/2014 09:09 GMT
2014-12-16 09:07:45,086 [myid:] - INFO  [main:Environment@100] - Client environment:host.name=precise64
2014-12-16 09:07:45,087 [myid:] - INFO  [main:Environment@100] - Client environment:java.version=1.6.0_30
2014-12-16 09:07:45,090 [myid:] - INFO  [main:Environment@100] - Client environment:java.vendor=Sun Microsystems Inc.
2014-12-16 09:07:45,091 [myid:] - INFO  [main:Environment@100] - Client environment:java.home=/usr/lib/jvm/java-6-openjdk-amd64/jre
2014-12-16 09:07:45,091 [myid:] - INFO  [main:Environment@100] - Client environment:java.class.path=/usr/local/zookeeper/bin/../build/classes:/usr/local/zookeeper/bin/../build/lib/*.jar:/usr/local/zookeeper/bin/../lib/slf4j-log4j12-1.6.1.jar:/usr/local/zookeeper/bin/../lib/slf4j-api-1.6.1.jar:/usr/local/zookeeper/bin/../lib/netty-3.7.0.Final.jar:/usr/local/zookeeper/bin/../lib/log4j-1.2.16.jar:/usr/local/zookeeper/bin/../lib/jline-0.9.94.jar:/usr/local/zookeeper/bin/../zookeeper-3.4.6.jar:/usr/local/zookeeper/bin/../src/java/lib/*.jar:/usr/local/zookeeper/bin/../conf:
2014-12-16 09:07:45,091 [myid:] - INFO  [main:Environment@100] - Client environment:java.library.path=/usr/lib/jvm/java-6-openjdk-amd64/jre/lib/amd64/server:/usr/lib/jvm/java-6-openjdk-amd64/jre/lib/amd64:/usr/lib/jvm/java-6-openjdk-amd64/jre/../lib/amd64:/usr/java/packages/lib/amd64:/usr/lib/jni:/lib:/usr/lib
2014-12-16 09:07:45,092 [myid:] - INFO  [main:Environment@100] - Client environment:java.io.tmpdir=/tmp
2014-12-16 09:07:45,092 [myid:] - INFO  [main:Environment@100] - Client environment:java.compiler=<NA>
2014-12-16 09:07:45,093 [myid:] - INFO  [main:Environment@100] - Client environment:os.name=Linux
2014-12-16 09:07:45,096 [myid:] - INFO  [main:Environment@100] - Client environment:os.arch=amd64
2014-12-16 09:07:45,097 [myid:] - INFO  [main:Environment@100] - Client environment:os.version=3.2.0-23-generic
2014-12-16 09:07:45,098 [myid:] - INFO  [main:Environment@100] - Client environment:user.name=hduser
2014-12-16 09:07:45,098 [myid:] - INFO  [main:Environment@100] - Client environment:user.home=/home/hduser
2014-12-16 09:07:45,099 [myid:] - INFO  [main:Environment@100] - Client environment:user.dir=/usr/local/zookeeper/conf
2014-12-16 09:07:45,101 [myid:] - INFO  [main:ZooKeeper@438] - Initiating client connection, connectString=localhost:2181 sessionTimeout=30000 watcher=org.apache.zookeeper.ZooKeeperMain$MyWatcher@4594a0ad
```

## Reference

- [ZooKeeper Getting Started Guide](https://zookeeper.apache.org/doc/r3.1.2/zookeeperStarted.html)



