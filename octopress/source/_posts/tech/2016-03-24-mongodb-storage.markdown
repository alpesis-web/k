---
layout: post_tech
title: "MongoDB Storage"
date: 2016-03-24 10:59:08 +0800
comments: true
categories: [tech]
tags: [db, mongo]
toc: true
---

## Definitions

- storage engine: the primary component of MongoDB responsible for managing data.
- journal: a log that helps the database recover in the event of a hard shutdown.
- GridFS: a versatile storage system that is suited to handling large files, such as those exceeding the 16 MB document size limit.


## Concepts

### Storage Engine

- WiredTiger: the default storage engine. It is well-suited for most workloads and is recommended for new deployments. WiredTiger provides a document-level concurrency model, checkpointing, and compression, among other features.
- MMAPv1: the original storage engine. It performs well on workloads with high volumes of reads and writes, as well as in-place updates.
- In-Memory Storage Engine: rather than storing documents on-disk, it retains them in-memory for more predictable data latencies. This storage engine is in beta.


### Journal

### GridFS

## Tutorials

## Reference

- [Storage](https://docs.mongodb.org/manual/storage/)
