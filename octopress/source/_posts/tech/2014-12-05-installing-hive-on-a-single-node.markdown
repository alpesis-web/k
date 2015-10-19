---
layout: post_tech
title: "Installing Hive on A Single Node"
date: 2014-12-05 10:55:40 +0800
comments: true
categories: [tech]
tags: [hive, hadoop, cluster]
toc: true
---

## Requirements

- Java 1.7
- Hadoop 2.x (preferred), 1.x.
- Hive versions up to 0.13 also supported Hadoop 0.20.x, 0.23.x.

Note:

- Hive versions 1.2 onward require Java 1.7 or newer. 
Hive versions 0.14 to 1.1 work with Java 1.6 as well. 
Users are strongly advised to start moving to Java 1.8 (see HIVE-8607).)
- Hive is commonly used in production Linux and Windows environment. 
Mac is a commonly used development environment. 
The instructions in this document are applicable to Linux and Mac. 
Using it on Windows would require slightly different steps.  

## 1. Installation

download sources

```
# official site
wget http://apache.claz.org/hive/stable/http://apache.claz.org/hive/stable/apache-hive-0.13.0-bin.tar.gz
# other
wget http://mirrors.oschina.net/apache/hive/stable/apache-hive-0.13.1-bin.tar.gz
```

install

```bash
$ tar xfz apache-hive-0.13.1-bin.tar.gz
$ sudo mv apache-hive-0.13.1-bin /usr/local/hive

$ sudo chown -R hduser:hadoop /usr/local/hive
```

## 2. Configuration

### 2.1. .bashrc

```bash
$ sudo nano ~/.bashrc
```

.bashrc

```bash
# Set HIVE_HOME
export HIVE_HOME="/usr/local/hive" 
PATH=$PATH:$HIVE_HOME/bin
export PATH
```

### 2.2. hive-config.sh

```bash
$ cd /usr/local/hive/bin
$ sudo nano hive-config.sh
```

hive-config.sh

```bash
# Allow alternate conf dir location.
HIVE_CONF_DIR="${HIVE_CONF_DIR:-$HIVE_HOME/conf" 
export HIVE_CONF_DIR=$HIVE_CONF_DIR
export HIVE_AUX_JARS_PATH=$HIVE_AUX_JARS_PATH

# Add the HADOOP_HOME
export HADOOP_HOME=/usr/local/hadoop    (write the path where hadoop file is there)
```

## 3. Testing

create hdfs folder

```bash
$ hadoop fs -mkdir       /tmp
$ hadoop fs -chmod g+w   /tmp

$ hdfs dfs -mkdir -p /user/hive/warehouse
$ hdfs dfs -chmod g+w  /user/hive/warehouse
```

run

```bash
$ hive


hduser@precise64:/usr/local/hive/bin$ hive

Logging initialized using configuration in jar:file:/usr/local/hive/lib/hive-common-0.13.1.jar!/hive-log4j.properties
hive>
```

## References

- [Hive Installation](http://doctuts.readthedocs.org/en/latest/hive.html#id1)
- [Hive Getting Started](https://cwiki.apache.org/confluence/display/Hive/GettingStarted)
