---
layout: post_tech
title: "MongoDB Replication"
date: 2016-03-24 10:59:23 +0800
comments: true
categories: [tech]
tags: [db, mongo]
toc: true
---

## Definitions

Replication is the process of synchronizing data across multiple servers.

Purposes of replication

- provides redundancy and increases data availability. With multiple copies of data on different database servers, replication provides a level of fault tolerance against the loss of a signle database server.
- provides increased read capacity as clients can send read operations to different servers. Maintaining copies of data in different data centers can increase data locality and availability for distributed applications.
- dedicated purposes, such as disaster recovery, reporting, or backup.



## Concepts

### Replica Set: Members

### Replica Set: Deployment Architectures

### Replica Set: High Availability

### Replica Set: Read and Write Senmantics

<img src="https://docs.mongodb.org/manual/_images/crud-write-concern-w2.png" />

image source: MongoDB official site

<img src="https://docs.mongodb.org/manual/_images/replica-set-read-preference.png" />

image source: MongoDB official site

### Replication Processes

### Master Slave Replication

## Tutorials

## Reference 

- [Replication](https://docs.mongodb.org/manual/core/replication-introduction/)
