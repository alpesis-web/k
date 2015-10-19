---
layout: post_tech
title: "Installing Sqoop on A Single Node"
date: 2014-12-07 17:33:32 +0800
comments: true
categories: [tech]
tags: [sqoop, hadoop, cluster]
toc: true
---

## Requirements

- Hadoop
- Hive
- Zookeeper
- HBase
- MySQL Driver for Java

## 1. Installation

```bash
$ sudo wget http://mirrors.oschina.net/apache/sqoop/1.4.4/sqoop-1.4.4.bin__hadoop-2.0.4-alpha.tar.gz
$ sudo tar xzf sqoop-1.4.4.bin__hadoop-2.0.4-alpha.tar.gz
$ sudo mv sqoop-1.4.4.bin__hadoop-2.0.4-alpha.tar.gz /usr/local/sqoop

$ sudo chown -R hduser:hadoop /usr/local/sqoop
$ sudo chmod 755 /usr/local/sqoop
```

## 2. Configuration

- .bashrc
- sqoop-env.sh
- configure-sqoop

### 2.1. .bashrc

```bash
$ sudo nano $HOME/.bashrc
$ source $HOME/.bashrc
```

.bashrc

```bash
# Set SQOOP_HOME
export SQOOP_HOME=/usr/local/sqoop
export PATH=$PATH:$SQOOP_HOME/bin
export CLASSPATH=$CLASSPATH:$SQOOP_HOME/lib
```


### 2.2. sqoop-env.sh

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

### 2.3. configure-sqoop

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

## 3. MySQL Driver

If database is MySQl, copy mysql-connector-java-5.1.25-bin.jar to `/usr/local/sqoop/lib`

download: [mysql-connector-java-5.1.25-bin.jar](http://yun.baidu.com/share/link?shareid=903445453&uk=3826203270)


## 4. Testing

```bash
$ sqoop help
```

outputs

```bash
hduser@precise64:/usr/local/sqoop/conf$ sqoop help
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

## Reference

- [介绍一下单机安装sqoop的过程](http://yuanxiaolong.gitcafe.com/blog/2014/08/17/install-sqoop-on-local/)
