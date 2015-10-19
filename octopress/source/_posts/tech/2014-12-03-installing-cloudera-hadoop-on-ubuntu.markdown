---
layout: post_tech
title:  "Installing Cloudera Hadoop on Ubuntu"
date:   2014-12-03 11:10:00 +0800
comments: true
categories: [tech]
tags: [cloudera, hadoop, linux, ubuntu, cluster]
---

NOTE: All hosts should be finished all the steps listed below.

## Preparation

hosts

```
202.120.83.101 insight-master
202.120.83.102 insight-slave1
202.120.83.103 insight-slave2
202.120.83.104 insight-slave3
202.120.83.105 insight-slave4
```

updating softwares

```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get dist-upgrade
```

modifing the hosts

```bash
$ sudo nano /etc/hosts
```

hosts

```
127.0.0.1       localhost
#127.0.1.1      insight

# Cloudera-Hadoop Cluster
202.120.83.101 insight-master
202.120.83.102 insight-slave1
202.120.83.103 insight-slave2
202.120.83.104 insight-slave3
202.120.83.105 insight-slave4
```

ssh

```bash
$ sudo apt-get install ssh

$ ssh-keygen -t rsa

$ # copy the publick key
$ cat /home/insight/.ssh/id_rsa.pub
$ sudo nano /home/insight/.ssh/authorized_keys
```

authorized_keys

```bash
# 202.120.83.101
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC+JcGP7MopXAdZUJR1AK7tCXHELACXh++6kxUf1FCAwdooJ8MlLiqMJTgCCSNMghiB0w442XB+lohQmFPDTLJDRaTZxbCvo82aD0sJ1slaxZDuu5/B3hv+pO7Nz4P1CHLw6JHsY+ml48GjX2FhI/iYDFg0RyEt3d91f+Bha9nubB3+uh9MGhAfpxsxMlc1J4/GTm6F1YfGV5aVfrztEM07bqVVaMS6tT0wrp5L57IzBhdcTOhd9asEmO/OpCQLytoIFS5kTv+M6bTU23ChMj7Df4BNEr2idwqUI/h3zx/jT25LR6qryWiwxZ2jho1yYUMM3jN6LqKSKXOarcrwgjOX insight@insight

# 202.120.83.102
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDHa6O+7kg+T6+G5UMhiPqG9PLcNu0LvO5VPIsuTMbWaer8w8ZRJE2R1KMlXmowMpiADFxeGWUEhruxyDp16JFv/axYc67dfvS1SSJiuNnAZ5PDQ7EIbPcOZqP7641XWY1kHrYbBr4gWnb/EectDQAzdb3GScWlEAu9/vamu7baHfm6C2X+0o3m3nkfB2i/Fd35TGL0xMJx3H/oOcX7Jm8MqCSgdJwIqBxilfqRrHC4VICYB2zDmzFNpgWEPlZZ0do1BKXOBddKMU7c0tgRCcuPLtMWJkIrEZIR7K9UGY7GniXA2Zq8GytjZj1215CyXaLQD9XhC5f0td3KxgmWCzxN insight@insight

# 202.120.83.103
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDeICFND0O2JSt7lXo7r8cFiqhFpZ9Vbc9AG2yQau6xQPvbwPZd6xtCljUWMTAMhdKOFsJp6TbYPWQr25HYUxw+7VUziN1dyXrfchRB4uBQqvYmwcay2vBJlZRAfEK1Ahm6LumXi+kozmHm+7SjBhY/24IRxLf+G1g7ldNW3jLx+GNXtHffH9mw6Uc/ihV/AaFHThR1s4j28mLTBMwVxAHSXadwN863u/hd9rsES7vpxKRNK6U3TymPE5gIRPkXTO5xoxoGKX9E8ClKjl7/D1/sOJI0WZWhlslhSQDGLpMLU1uwK8RT5f4saqZyOv9kva7ad1gIYXXibmhzuct7sk67 insight@insight

# 202.120.83.104
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCbDG89bL9yNik01Xo2fhf8QHh1MzWcvIRIfK1GyGOh5b8LvaE3iZtX2RKYI4CCzvJ6y3A8peTWUuRZSV0lO7cpyOIoFIFRHScSuLulv8dHj0gYyjsH66R6lbnFYk72Ih/XzRUSH8Fo4Ar+eXWVZfBzWZWTL3ftRR7fmc5mZP+4IH7LdgZI2Wgb/GwksCx09vlMQmiQzHIYRs3iD6fsfL5prvcPKHYmwqslZcrBuy9nW2s0un3yRTeWwxYgRT15CHKXeOq5N+H0WxORCkY7FvLttPQbaAhbldvnir3NaIwCnds7vOvry5dm79LJzIC+2br7WQ2spUGCRtrzw8IcO4N9 insight@insight

# 202.120.83.105
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDAjeDr/HcUsjs7xq7MlpFnGbr/hDr9SaQhet4Fp8UQnQKwuFVtUqP48Dwr0sVYY8xueqhylI2q9wL+rovIk2T1DKeD+z0BX9P1C3CrcHNMpirEotdQd2OGGwsIEMTeMugox1oER3LWLjGBcxZ71KOx2INSxBq/6EoDSOhMIbVH0ZC9fPdG/mEScf3VmvzBT1UwyewZVoVPqSr6NmOJ1RVxKA9c0P7hHtBTcUhwFNLgitM8ca6Y4eL6DsTuIl3zQA+dSnpa8IudCY1oARnphQInHIa9wCrnMCAEgRqPHfyRwg8hcOrOerWEq2Q95m15dPoIrmfy8T63p0zrhKXN34lJ insight@insight
```

validation

```bash
$ ssh 202.120.83.xxx
```

## MySQL

installation

```bash
$ sudo apt-get install mysql-server libmysql-java
```

configuration

```bash
$ sudo nano /etc/mysql/conf.d/mysql_cloudera_manager.cnf
```

mysql_cloudera_manager.cnf

```bash
[mysqld]
transaction-isolation=READ-COMMITTED
# Disabling symbolic-links is recommended to prevent assorted security risks;
# to do so, uncomment this line:
# symbolic-links=0

key_buffer              = 16M
key_buffer_size         = 32M
max_allowed_packet      = 16M
thread_stack            = 256K
thread_cache_size       = 64
query_cache_limit       = 8M
query_cache_size        = 64M
query_cache_type        = 1
# Important: see Configuring the Databases and Setting max_connections
max_connections         = 550

# log-bin should be on a disk with enough free space
log-bin=/var/log/mysql/mysql_binary_log

# For MySQL version 5.1.8 or later. Comment out binlog_format for older versions.
binlog_format           = mixed

read_buffer_size = 2M
read_rnd_buffer_size = 16M
sort_buffer_size = 8M
join_buffer_size = 8M

# InnoDB settings
innodb_file_per_table = 1
innodb_flush_log_at_trx_commit  = 2
innodb_log_buffer_size          = 64M
innodb_buffer_pool_size         = 4G
innodb_thread_concurrency       = 8
innodb_flush_method             = O_DIRECT
innodb_log_file_size = 512M

[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
```

configurating my.cnf

```bash
sudo nano /etc/mysql/my.cnf

# comment the bind-address
# bind-address   = 127.0.0.1
```

configurating innodb

```bash
$ sudo su
$ mv /var/lib/mysql/ib_logfile* /var/tmp/
$ exit
```

initialization

```bash
$ sudo service mysql restart
$ mysql -uroot -p

mysql> create database amon DEFAULT CHARACTER SET utf8;
mysql> grant all on amon.* TO 'amon'@'%' IDENTIFIED BY 'amon_password';
mysql> grant all on amon.* TO 'amon'@'insight-master' IDENTIFIED BY 'amon_password';
mysql> create database smon DEFAULT CHARACTER SET utf8;
mysql> grant all on smon.* TO 'smon'@'%' IDENTIFIED BY 'smon_password';
mysql> grant all on smon.* TO 'smon'@'insight-master' IDENTIFIED BY 'smon_password';
mysql> create database rman DEFAULT CHARACTER SET utf8;
mysql> grant all on rman.* TO 'rman'@'%' IDENTIFIED BY 'rman_password';
mysql> grant all on rman.* TO 'rman'@'insight-master' IDENTIFIED BY 'rman_password';
mysql> create database hmon DEFAULT CHARACTER SET utf8;
mysql> grant all on hmon.* TO 'hmon'@'%' IDENTIFIED BY 'hmon_password';
mysql> grant all on hmon.* TO 'hmon'@'insight-master' IDENTIFIED BY 'hmon_password';
mysql> create database hive DEFAULT CHARACTER SET utf8;
mysql> grant all on hive.* TO 'hive'@'%' IDENTIFIED BY 'hive_password';
mysql> grant all on hive.* TO 'hive'@'insight-master' IDENTIFIED BY 'hive_password';

mysql> create database nav DEFAULT CHARACTER SET utf8;
mysql> grant all on nav.* TO 'nav'@'%' IDENTIFIED BY 'nav_password';
mysql> grant all on nav.* TO 'nav'@'insight-master' IDENTIFIED BY 'nav_password';

mysql> create database navms DEFAULT CHARACTER SET utf8;
mysql> grant all on navms.* TO 'navms'@'%' IDENTIFIED BY 'navms_password';
mysql> grant all on navms.* TO 'navms'@'insight-master' IDENTIFIED BY 'navms_password';
```

## Cloudera Manager

installing java

```bash
$ sudo apt-get install python-software-properties
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update

$ sudo nano /etc/apt/sources.list.d/cloudera.list
```

cloudera.list

```bash
deb [arch=amd64] http://archive.cloudera.com/cdh5/ubuntu/precise/amd64/cdh precise-cdh5 contrib
deb-src http://archive.cloudera.com/cdh5/ubuntu/precise/amd64/cdh precise-cdh5 contrib
deb [arch=amd64] http://archive.cloudera.com/cm5/ubuntu/precise/amd64/cm precise-cm5 contrib
deb-src http://archive.cloudera.com/cm5/ubuntu/precise/amd64/cm precise-cm5 contrib
```

adding key

```bash
$ curl -s http://archive.cloudera.com/cdh5/ubuntu/precise/amd64/cdh/archive.key | sudo apt-key add -
$ sudo apt-get update
```

installing manager by manual

```bash
$ sudo apt-get -q -y --force-yes install oracle-j2sdk1.7 cloudera-manager-server-db cloudera-manager-server cloudera-manager-daemons cloudera-manager-agent

$ # config cloudera-manager-server db
$ sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql  -uroot -p --scm-host localhost scm scm scm_password

$ # start service
$ service cloudera-scm-server-db initdb
$ service cloudera-scm-server-db start
$ service cloudera-scm-server start
```

installing manager automatically

```bash
$ sudo ./cloudera-mangaer-installer.bin
```

steps:

```bash
$ apt-get -q -y --force-yes install oracle-j2sdk1.7 cloudera-manager-server-db cloudera-manager-server cloudera-manager-daemons
$ service cloudera-scm-server-db initdb
$ service cloudera-scm-server-db start
$ service cloudera-scm-server start
```

## Cloudera Agent

installing java

```bash
$ sudo apt-get install python-software-properties
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-j2sdk1.7
```

adding sources

```bash
$ sudo nano /etc/apt/sources.list.d/cloudera.list
```

cloudera.list

```bash
deb [arch=amd64] http://archive.cloudera.com/cdh5/ubuntu/precise/amd64/cdh precise-cdh5 contrib
deb-src http://archive.cloudera.com/cdh5/ubuntu/precise/amd64/cdh precise-cdh5 contrib
deb [arch=amd64] http://archive.cloudera.com/cm5/ubuntu/precise/amd64/cm precise-cm5 contrib
deb-src http://archive.cloudera.com/cm5/ubuntu/precise/amd64/cm precise-cm5 contrib
```

adding keys

```bash
$ curl -s http://archive.cloudera.com/cdh5/ubuntu/precise/amd64/cdh/archive.key | sudo apt-key add -
$ sudo apt-get update
```

installing agent

```bash
$ sudo apt-get install cloudera-manager-agent cloudera-manager-daemons
```

configurating agent

```bash
$ sudo nano /etc/cloudera-scm-agent/config.ini
```

config.ini

```bash
server_host=insight-master  # master host name
```

start services

```bash
$ sudo service cloudera-scm-agent  restart
```


## Cloudera Hadoop


restart agents on slaves

```bash
$ sudo service cloudera-scm-agent  restart
```

restart master 

```bash
$ sudo service cloudera-scm-server restart
$  sudo service cloudera-scm-agent  restart
```

open the link on browser:

- http://202.120.83.101:7180
- username/password: admin/admin

steps:

- step1. login

- step2. welcome

- step3. select hosts

- step4. express-wizard

- step5. install parcel

- step6. host inspector

updating swappiness

```bash
vagrant ssh insight-master/insight-slave1/insight-slave2/insight-slave3/insight-slave4 # for all hosts
sudo nano /proc/sys/vm/swappiness

# modification
60 -> 0
```

- step7. select services

- step8. assign roles

- step9. config db

- step10. review steps

- step11. start services

- step12. done


