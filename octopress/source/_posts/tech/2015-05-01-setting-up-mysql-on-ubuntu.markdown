---
layout: post_tech
title:  "Setting Up MySQL on Ubuntu"
date:   2015-05-01 10:50:00 +0800
comments: true
categories: [tech]
tags: [mysql, ubuntu]
---

installation

```bash
$ sudo apt-get install mysql-server libmysql-java
```

configuration

```bash
$ sudo nano /etc/mysql/my.cnf
    
# comment bind-address
# bind-address = 127.0.0.1
```

configurating database

```bash
$ sudo service mysql restart
$ mysql -uroot -p

$ create database analytics DEFAULT CHARACTER SET utf8;
$ grant all on analytics.* TO 'insight'@'%' IDENTIFIED BY 'analytics_password';
$ grant all on analytics.* TO 'insight'@'202.120.83.105' IDENTIFIED BY 'analytics_password';
$ grant all on analytics.* TO 'insight'@'202.120.83.101' IDENTIFIED BY 'analytics_password';

$ GRANT ALL ON *.* TO insight@'localhost'  IDENTIFIED BY 'insight' WITH GRANT OPTION;

$ exit;
```
