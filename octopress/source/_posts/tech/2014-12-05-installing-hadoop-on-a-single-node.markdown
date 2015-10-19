---
layout: post_tech
title: "Installing Hadoop on A Single Node"
date: 2014-12-05 02:40:54 +0800
comments: true
categories: [tech]
tags: [hadoop, cluster, java]
toc: true
---

## Requirements

- Ubuntu 12.04
- SSH
- Java
- Hadoop

It will install Apache Hadoop on a single node, the services are including

- Hadoop
- HDFS
- YARN / MapReduce

## 1. Ubuntu

### 1.1. SSH

```bash
$ sudo apt-get install ssh-server
$ sudo addgroup hadoop
$ sudo adduser --ingroup hadoop hduser

$ su - hduser
$ ssh-keygen -t rsa -P "" 
$ cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys

$ ssh localhost
```


### 1.2. Config User

```bash
$ su vagrant
$ sudo visudo

# add sudoer
hduser ALL=(ALL:ALL) ALL
```

### 1.3. IPv6

disable IPv6

```bash
su hduser
sudo nano /etc/sysctl.conf
```

sysctl.conf

```
# disable ipv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

check 

```bash
$ cat /proc/sys/net/ipv6/conf/all/disable_ipv6
```

note: 0 is enable, 1 is disable.

## 2. Java

```bash
$ apt-get update
$ apt-get install default-jdk

$ java -version
```


## 3. Hadoop Installation

```bash
# source
wget http://www.motorlogy.com/apache/hadoop/common/current/hadoop-2.6.0.tar.gz
wget http://mirrors.cnnic.cn/apache/hadoop/common/stable/hadoop-2.6.0.tar.gz
```

install

```bash
$ tar xfz hadoop-2.6.0.tar.gz
$ sudo mv hadoop-2.6.0 /usr/local/hadoop
```

## 4. Hadoop Configuration

config files

- ~/.bashrc
- /usr/local/hadoop/etc/hadoop/hadoop-env.sh
- /usr/local/hadoop/etc/hadoop/core-site.xml
- /usr/local/hadoop/etc/hadoop/yarn-site.xml
- /usr/local/hadoop/etc/hadoop/mapred-site.xml
- /usr/local/hadoop/etc/hadoop/hdfs-site.xml

### 4.1. .bashrc

```bash
$ update-alternatives --config java

$ sudo nano ~/.bashrc
$ source ~/.bashrc
```

.bashrc

```bash .bashrc
#HADOOP VARIABLES START
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
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

### 4.2. hadoop-env.sh

```bash
$ sudo nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh
```

hadoop-env.sh

```bash
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
```

### 4.3. core-site.xml

```bash
sudo mkdir -p /app/hadoop/tmp
sudo chown hduser:hadoop /app/hadoop/tmp
sudo chmod 750 /app/hadoop/tmp

# or
sudo chown hduser:hadoop /app/hadoop
```

NOTE: if not config above, it will flag `java.io.IOException` error.

```bash
$ sudo nano /usr/local/hadoop/etc/hadoop/core-site.xml
```

core-site.xml

```xml core-site.xml
<property>
  <name>hadoop.tmp.dir</name>
  <value>/app/hadoop/tmp</value>
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

### 4.4. yarn-site.xml

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

### 4.5. mapred-site.xml

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


### 4.6. hdfs-site.xml

create the folders for namenode and datanode

```bash
$ mkdir -p /usr/local/hadoop_store/hdfs/namenode
$ mkdir -p /usr/local/hadoop_store/hdfs/datanode
$ sudo chown -R hdusr:hadoop /usr/local/hadoop_store
```

config hdfs-site.xml

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
```

## 5. HDFS

formatting HDFS

```bash
$ hdfs namenode -format
```

## 5. Services

```bash
start-all.sh  # or start-dfs.sh and start-yarn.sh
start-dfs.sh
start-yarn.sh

jps

stop-all.sh # close all services
stop-dfs.sh
stop-yarn.sh
```

check the ports

```bash
$ sudo netstat -plten | grep java
```

## References

- [Hadoop Official Site](http://hadoop.apache.org/docs/r1.2.1/single_node_setup.html)
- [How to Install Hadoop on Ubuntu 13.10](https://www.digitalocean.com/community/tutorials/how-to-install-hadoop-on-ubuntu-13-10)
- [running-hadoop-on-ubuntu-linux-single-node-cluster](http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/)
- [Hadoop MapReduce Next Generation - Setting up a Single Node Cluster](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html)

