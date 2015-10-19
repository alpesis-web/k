---
layout: post_tech
title: "Creating A Hadoop Box"
date: 2014-12-15 12:48:56 +0800
comments: true
categories: [tech]
tags: [vagrant, virtualbox, hadoop, cluster, hive, sqoop, hbase, zookeeper]
toc: true
---

This tutorial is about installing apache hadoop by manual in a virtual machine, 
and then export the hadoop-box, others can import the box directly without other 
extra works.

The cluster services including:

- Hadoop
- HDFS
- Yarn
- Hive
- HBase
- Zookeeper
- Sqoop

## Requirements

- Ubuntu 12.04
- Java
- Vagrant / VirtualBox
- Hadoop
- Hive
- HBase
- Zookeeper
- Sqoop


## 1. Vagrant / VirtualBox

First of all, install VirtualBox and Vagrant in the local machine, 
and then download the `precise64.box`.

Once done, create a new virtual machine for the box.


```bash
$ vagrant box add precise64 precise64.box

$ mkdir hadoop-stack
$ cd hadoop-stack
$ vagrant init precise64
$ vagrant up
``` 

## 2. Ubuntu 12.04

### 2.1. Java

```bash
$ apt-get update
$ apt-get install default-jdk

$ java -version
```

### 2.2. SSH

```bash
$ sudo apt-get install ssh-server

$ su - vagrant
$ ssh-keygen -t rsa -P "" 
$ cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys

$ ssh localhost
```

### 2.3. sysctl

```bash
$ sudo nano /etc/sysctl.conf
```

sysctl.conf

```
# disable ipv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

disable IPv6

```bash
$ cat /proc/sys/net/ipv6/conf/all/disable_ipv6
```

NOTE: 0 is enable, 1 is disable

## 3. Cluster

Steps:

- Hadoop
- Hive
- Zookeeper
- HBase
- Sqoop

## 4. Hadoop

### 4.1. installation

```bash
$ wget http://www.motorlogy.com/apache/hadoop/common/current/hadoop-2.6.0.tar.gz
# or
$ wget http://mirrors.cnnic.cn/apache/hadoop/common/stable/hadoop-2.6.0.tar.gz

$ tar xfz hadoop-2.6.0.tar.gz
$ sudo mv hadoop-2.6.0 /usr/local/hadoop

$ sudo chown -R vagrant:vagrant /usr/local/hadoop
$ sudo chmod 750 /usr/local/hadoop
```

### 4.2. .bashrc

```bash
$ update-alternatives --config java
$ sudo nano $HOME/.bashrc
$ source $HOME/.bashrc
```

.bashrc

```bash .bashrc
#HADOOP VARIABLES START
export JAVA_HOME=/usr/lib/jvm/java-6-openjdk-amd64
export HADOOP_INSTALL=/usr/local/hadoop
export PATH=$PATH:$HADOOP_INSTALL/bin
export PATH=$PATH:$HADOOP_INSTALL/sbin
export HADOOP_MAPRED_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_HOME=$HADOOP_INSTALL
export HADOOP_HDFS_HOME=$HADOOP_INSTALL
export YARN_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_INSTALL/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_INSTALL/lib" 
#HADOOP VARIABLES END

# Some convenient aliases and functions for running Hadoop-related commands
unalias fs &> /dev/null
alias fs="hadoop fs" 
unalias hls &> /dev/null
alias hls="fs -ls" 

# If you have LZO compression enabled in your Hadoop cluster and
# compress job outputs with LZOP (not covered in this tutorial):
# Conveniently inspect an LZOP compressed file from the command
# line; run via:
#
# $ lzohead /hdfs/path/to/lzop/compressed/file.lzo
#
# Requires installed 'lzop' command.
#
lzohead () {
    hadoop fs -cat $1 | lzop -dc | head -1000 | less
}
```

### 4.3. hadoop-env.sh

```bash
$ sudo nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh
```

hadoop-env.sh

```bash hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/java-6-openjdk-amd64
```

### 4.4. core-site.xml

```bash
$ sudo nano /usr/local/hadoop/etc/hadoop/core-site.xml
```

core-site.xml

```xml core-site.xml
<property>
  <name>hadoop.tmp.dir</name>
  <value>/hdfs/hadoop/tmp</value>
  <description>A base for other temporary directories.</description>
</property>

<property>
  <name>fs.default.name</name>
  <value>hdfs://localhost:54310</value>
  <description>The name of the default file system.  A URI whose
  scheme and authority determine the FileSystem implementation.  The
  uri's scheme determines the config property (fs.SCHEME.impl) naming
  the FileSystem implementation class.  The uri's authority is used to
  determine the host, port, etc. for a filesystem.</description>
</property>
```

create HDFS

```bash
$ sudo mkdir -p /hdfs/hadoop/tmp
$ sudo chown vagrant:vagrant /hdfs/hadoop/tmp
$ sudo chmod 750 /hdfs/hadoop/tmp
```

### 4.5. yarn-site.xml

```bash
$ sudo nano /usr/local/hadoop/etc/hadoop/yarn-site.xml
```

yarn-site.xml

```xml yarn-site.xml
<property>
   <name>yarn.nodemanager.aux-services</name>
   <value>mapreduce_shuffle</value>
</property>
<property>
   <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
   <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
```

### 4.6. mapred-site.xml

```bash
$ sudo cp /usr/local/hadoop/etc/hadoop/mapred-site.xml.template /usr/local/hadoop/etc/hadoop/mapred-site.xml
$ sudo nano /usr/local/hadoop/etc/hadoop/mapred-site.xml
```

mapred-site.xml

```xml mapred-site.xml
<property>
  <name>mapred.job.tracker</name>
  <value>localhost:54311</value>
  <description>The host and port that the MapReduce job tracker runs
  at.  If "local", then jobs are run in-process as a single map
  and reduce task.
  </description>
</property>
```

### 4.7. hdfs-site.xml

```bash
$ sudo nano /usr/local/hadoop/etc/hadoop/hdfs-site.xml
```

hdfs-site.xml

```xml hdfs-site.xml
<property>
  <name>dfs.replication</name>
  <value>1</value>
  <description>Default block replication.
  The actual number of replications can be specified when the file is created.
  The default is used if replication is not specified in create time.
  </description>
</property>

<property>
  <name>dfs.namenode.name.dir</name>
  <value>file:/hdfs/hadoop_store/hdfs/namenode</value>
</property>

<property>
  <name>dfs.datanode.data.dir</name>
  <value>file:/hdfs/hadoop_store/hdfs/datanode</value>
</property>
```

create HDFS

```bash
$ mkdir -p /hdfs/hadoop_store/hdfs/namenode
$ mkdir -p /hdfs/hadoop_store/hdfs/datanode
$ sudo chown -R vagrant:vagrant /hdfs/hadoop_store
$ sudo chmod 750 /hdfs/hadoop_store
```

### 4.8. Testing

```bash
$ hdfs namenode -format

$ start-all.sh  # replace start-dfs.sh and start-yarn.sh
$ start-dfs.sh
$ start-yarn.sh

$ jps

$ stop-all.sh 
$ stop-dfs.sh
$ stop-yarn.sh
```

services

- Jps
- NodeManager
- SecondaryNameNode
- NameNode
- ResourceManager
- DataNode

UI:

- http://localhost:50070/
- http://localhost:8088/

## 5. Hive


### 5.1. Installation

```bash
$ wget http://mirrors.cnnic.cn/apache/hive/stable/apache-hive-0.14.0-bin.tar.gz

$ tar xfz apache-hive-0.14.0-bin.tar.gz
$ sudo mv apache-hive-0.14.0-bin.tar.gz /usr/local/hive

$ sudo chown -R vagrant:vagrant /usr/local/hive
$ sudo chmod 750 /usr/local/hive
```

### 5.2. .bashrc

```bash
$ sudo nano $HOME/.bashrc
$ source $HOME/.bashrc
```

.bashrc

```bash .bashrc
# Set HIVE_HOME
export HIVE_HOME="/usr/local/hive" 
PATH=$PATH:$HIVE_HOME/bin
export PATH
```

### 5.3. hive-config.sh

```bash
$ sudo nano /usr/local/hive/bin/hive-config.sh
```

hive-config.sh

```bash hive-config.sh
# Allow alternate conf dir location.
HIVE_CONF_DIR="${HIVE_CONF_DIR:-$HIVE_HOME/conf" 
export HIVE_CONF_DIR=$HIVE_CONF_DIR
export HIVE_AUX_JARS_PATH=$HIVE_AUX_JARS_PATH

# add HADOOP_HOME
export HADOOP_HOME=/usr/local/hadoop    (write the path where hadoop file is there)
```

### 5.4. create HDFS folders

```bash
$ hadoop fs -mkdir       /tmp
$ hadoop fs -chmod g+w   /tmp

$ hdfs dfs -mkdir -p /user/hive/warehouse
$ hdfs dfs -chmod g+w  /user/hive/warehouse
```

### 5.5. Testing

```bash
vagrant@precise64:/usr/local/hive/bin$ hive

Logging initialized using configuration in jar:file:/usr/local/hive/lib/hive-common-0.13.1.jar!/hive-log4j.properties
hive>
```


## 6. Zookeeper

### 6.1. Installation

```bash
$ sudo wget http://mirrors.cnnic.cn/apache/zookeeper/stable/zookeeper-3.4.6.tar.gz
$ sudo tar xzf zookeeper-3.4.6.tar.gz
$ sudo mv zookeeper-3.4.6 /usr/local/zookeeper

$ sudo chown -R vagrant:vagrant /usr/local/zookeeper
$ sudo chmod 750 /usr/local/zookeeper
```

### 6.2. .bashrc

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

### 6.3. zoo.cfg

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

### 6.4. Testing

```bash
$ zkServer.sh start
$ jps
$ zkCli.sh 127.0.0.1:2181
```

NOTE: if `QuorumPeerMain`, then succeed.


## 7. HBase

### 7.1. Installation

```bash
$ sudo wget http://mirrors.cnnic.cn/apache/hbase/stable/hbase-0.98.9-hadoop2-bin.tar.gz
$ sudo tar xzf hbase-0.98.9-hadoop2-bin.tar.gz
$ sudo mv hbase-0.98.9-hadoop2-bin.tar.gz /usr/local/hbase

$ sudo chown -R vagrant:vagrant /usr/local/hbase
$ sudo chmod 750 /usr/local/hbase
```

### 7.2. .bashrc

```bash
$ sudo nano $HOME/.bashrc
$ source $HOME/.bashrc
```

.bashrc

```bash .bashrc
# Set HBASE_HOME
export HBASE_HOME=/usr/local/hbase
export PATH=$PATH:$HBASE_HOME/bin
```

### 7.3. hbase-env.sh

```bash
$ sudo nano /usr/local/hbase/conf/hbase-env.sh
```

hbase-env.sh

```bash hbase-env.sh
export JAVA_HOME=/usr/lib/jvm/java-6-openjdk-amd64
```

### 7.4. hbase-site.xml

```bash
$ sudo nano /usr/local/hbase/conf/hbase-site.xml
```

hbase-site.xml

```xml hbase-site.xml
   <property>
       <name>hbase.rootdir</name>
       <value>hdfs://localhost:54310/user/hbase</value>            
       # NOTE: localhost must be the same as regisonserver
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

### 7.5. regionservers

```bash
$ sudo nano /usr/local/hbase/conf/regionservers
```

regionservers

```
localhost
```

### 7.6. HDFS

```bash
$ sudo mkdir /hdfs/zookeeper
$ sudo chown -R vagrant:vagrant /hdfs/zookeeper
$ sudo chmod 755 /hdfs/zookeeper
```

### 7.7. Testing


```bash
$ start-hbase.sh
$ jps
```

outputs

```bash
vagrant@precise64:/usr/local/hbase/conf$ jps
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

- if zookeeper enabled, `HMaster` and `HRegionServer`
- if zookeeper disabled, `HMaster`, `HRegionServer` and `HQuorumPeer`

```bash
$ hbase shell

vagrant@precise64:/usr/local/hbase/conf$ hadoop fs -ls /user/hbase
Found 5 items
drwxr-xr-x   - hduser supergroup          0 2014-12-18 16:31 /user/hbase/.tmp
drwxr-xr-x   - hduser supergroup          0 2014-12-18 16:31 /user/hbase/data
-rw-r--r--   1 hduser supergroup         42 2014-12-18 16:31 /user/hbase/hbase.id
-rw-r--r--   1 hduser supergroup          7 2014-12-18 16:31 /user/hbase/hbase.version
drwxr-xr-x   - hduser supergroup          0 2014-12-18 16:31 /user/hbase/oldWALs
```

UI

- HMaster: http://localhost:60010
- HRegionServer: http://localhost:60030

## 8. Sqoop

### 8.1. Installation

```bash
$ sudo wget http://mirrors.cnnic.cn/apache/sqoop/1.4.5/sqoop-1.4.5.bin__hadoop-2.0.4-alpha.tar.gz
$ sudo tar xzf sqoop-1.4.5.bin__hadoop-2.0.4-alpha.tar.gz
$ sudo mv sqoop-1.4.5.bin__hadoop-2.0.4-alpha.tar.gz /usr/local/sqoop

$ sudo chown -R vagrant:vagrant /usr/local/sqoop
$ sudo chmod 755 /usr/local/sqoop
```

### 8.2. .bashrc

```bash
$ sudo nano $HOME/.bashrc
$ source $HOME/.bashrc
```

.bashrc

```bash .bashrc
# Set SQOOP_HOME
export SQOOP_HOME=/usr/local/sqoop
export PATH=$PATH:$SQOOP_HOME/bin
export CLASSPATH=$CLASSPATH:$SQOOP_HOME/lib
```

### 8.3. sqoop-env.sh 

```bash
$ cd /usr/local/sqoop/conf
$ cp sqoop-env-template.sh   sqoop-env.sh
$ sudo nano sqoop-env.sh
```

sqoop-env.sh


```bash sqoop-env.sh
#Set path to where bin/hadoop is available
export HADOOP_COMMON_HOME=/usr/local/hadoop

#Set path to where hadoop-*-core.jar is available
export HADOOP_MAPRED_HOME=/usr/local/hadoop

#set the path to where bin/hbase is available
export HBASE_HOME=/usr/local/hbase

#Set the path to where bin/hive is available
export HIVE_HOME=/usr/local/hive

#Set the path for where zookeper config dir is
export ZOOCFGDIR=/usr/local/hbase/lib
```

### 8.4. configure-sqoop

```bash
$ cd /usr/local/sqoop/bin
$ sudo nano configure-sqoop
```


configure-sqoop

```bash configure-sqoop
#if [ -z "${HCAT_HOME}" ]; then
#  HCAT_HOME=/usr/lib/hcatalog
#fi

## Moved to be a runtime check in sqoop.
#if [ ! -d "${HCAT_HOME}" ]; then
#  echo "Warning: $HCAT_HOME does not exist! HCatalog jobs will fail." 
#  echo 'Please set $HCAT_HOME to the root of your HCatalog installation.'
#fi

# Add HCatalog to dependency list
#if [ -e "${HCAT_HOME}/bin/hcat" ]; then
#  TMP_SQOOP_CLASSPATH=${SQOOP_CLASSPATH}:`${HCAT_HOME}/bin/hcat -classpath`
#  if [ -z "${HIVE_CONF_DIR}" ]; then
#    TMP_SQOOP_CLASSPATH=${TMP_SQOOP_CLASSPATH}:${HIVE_CONF_DIR}
#  fi
#  SQOOP_CLASSPATH=${TMP_SQOOP_CLASSPATH}
#fi
```

### 8.5. MySQL Driver

if MySQL, copy `mysql-connector-java-5.1.25-bin.jar` to `/usr/local/sqoop/lib`

download: [mysql-connector-java-5.1.25-bin.jar](http://yun.baidu.com/share/link?shareid=903445453&uk=3826203270)


### 8.6. Testing

```bash
vagrant@precise64:/usr/local/sqoop/conf$ sqoop help
usage: sqoop COMMAND [ARGS]

Available commands:
  codegen            Generate code to interact with database records
  create-hive-table  Import a table definition into Hive
  eval               Evaluate a SQL statement and display the results
  export             Export an HDFS directory to a database table
  help               List available commands
  import             Import a table from a database to HDFS
  import-all-tables  Import tables from a database to HDFS
  job                Work with saved jobs
  list-databases     List available databases on a server
  list-tables        List available tables in a database
  merge              Merge results of incremental imports
  metastore          Run a standalone Sqoop metastore
  version            Display version information

See 'sqoop help COMMAND' for information on a specific command.
```

## 9. Export box

```bash
$ vagrant package --base <vm_id>

# for example
$ vagrant package --base vagrant_default_1404223024566_2093
```


