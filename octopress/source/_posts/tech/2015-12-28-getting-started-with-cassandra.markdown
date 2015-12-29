---
layout: post_tech
title: "Getting Started With Cassandra"
date: 2015-12-28 18:26:26 +0800
comments: true
categories: [tech]
tags: [cassandra, db]
toc: true
---

## 1. Installation

- Ubuntu 14.04
- Java 8
- Apache Cassandra

Java

```bash
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-java8-installer
$ java -version
$ sudo apt-get install oracle-java8-set-default
```

Cassandra

```bash
$ sudo vim /etc/apt/source.list.d/cassandra.list

# add source list
deb http://www.apache.org/dist/cassandra/debian 21x main
deb-src http://www.apache.org/dist/cassandra/debian 21x main

# add public key
$ gpg --keyserver pgp.mit.edu --recv-keys F758CE318D77295D
$ gpg --export --armor F758CE318D77295D | sudo apt-key add -

$ sudo apt-get update
$ sudo apt-get install cassandra
```


## 2. Configuration

config paths:

- data directories: /var/lib/cassandra
- log directory: /var/log/cassandra
- runtime files: /var/run/cassandra
- environment settings: /usr/share/cassandra
- JAR files: /usr/share/cassandra/lib
- binary files: /usr/bin
- /usr/sbin
- configuration files: /etc/cassandra
- service startup script: /etc/init.d
- cassandra user limits: /etc/security/limits.d
- /etc/default

## 3. Testing

running cassandra

```bash
$ sudo cassandra
$ sudo cassandra -f

$ cassandra-cli
```

running cql

```bash
$ sudo cqlsh
$ sudo cqlsh 127.0.0.1 9042
Connected to Test Cluster at 127.0.0.1:9042.
[cqlsh 5.0.1 | Cassandra 2.1.12 | CQL spec 3.2.1 | Native protocol v3]
Use HELP for help.

cqlsh> CREATE KEYSPACE test WITH REPLICATION = { 'class': 'SimpleStrategy', 'replication_factor': 1 }; 

cqlsh> USE test;
cqlsh:test> CREATE TABLE users (
        ... user_id int PRIMARY KEY,
        ... fname text,
        ... lname text
        ... );
cqlsh:test> INSERT INTO users (user_id, fname, lname) VALUES (1722, 'joe', 'lambert');
cqlsh:test> INSERT INTO users (user_id, fname, lname) VALUES (1832, 'alice', 'parker');

cqlsh:test> SELECT * FROM users;
cqlsh:test> CREATE INDEX ON users (lname);
cqlsh:test> SELECT * FROM users WHERE lname = 'smith';
```

## Trouble Shooting

### CQL Connection refused

error message

```bash
Connection error: ('Unable to connect to any servers', {'127.0.0.1': error(111, "Tried connecting to [('127.0.0.1', 9042)]. Last error: Connection refused")})
```

solution: update Java Version (Up to Java 8)
