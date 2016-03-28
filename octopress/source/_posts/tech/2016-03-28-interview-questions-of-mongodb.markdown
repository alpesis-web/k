---
layout: post_tech
title: "Interview Questions of MongoDB"
date: 2016-03-28 16:50:08 +0800
comments: true
categories: [tech]
tags: [db, mongo]
toc: true
---

## Concepts

### Define NoSQL.

A NoSQL database provides a mechanism for storage and retrieval of data that is modeled
in means other than the tabular relations used in relational database (like SQL, Oracle, etc.)

Types of NoSQL databases:

- key-value: Redis, Memcache
- column based: HBase, Cassandra
- document: MongoDB, ElasticSearch
- Graph: Neo4j

Differences between SQL databases and NoSQL databases:

- SQL databases store data in form of tables, rows, columns and records. This data is stored in a pre-defined data model which is not very much flexible for today's real-world highly growing
applications.
- MongoDB uses a flexible structure which can be easily modified and extended. MongoDB allows a
highly flexible and scalable document structure. For example, one data document in MongoDB can have five columns and the other one in the same collection can have ten columns. Also, MongoDB
database are faster as compared to SQL databases due to efficient indexing and storage techniques.


### Define MongoDB.

MongoDB is a document oriented database, it stores data in the form of BSON structure based
documents. These documents are stored in a collection.

Features:

- flexible data model in form of documents
- agile and highly scalable database
- faster than traditional databases
- expressive query language

### Define namespace.

A namespace is the concatenation of the database name and collection name.

```
school.students       # school - database, students - collection
```

### Define ObjectID.

ObjectID is a 12-byte BSON type with:

- 4 bytes value representing seconds
- 3 bytes machine identifier
- 2 bytes process id
- 3 bytes counter


## Database Design


### Define embeded documents.

When should consider embeding documents:

- 'contains' relationships between entities
- one-to-many relationships
- performance reasons


## Database Languages

### Define indexes.

Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB
must perform a collection scan, i.e., scan every document in a collection, to select those
documents that match the query statement. If an appropriate index exists for a query, MongoDB
can use the index to limit the number of documents it must inspect.

Types of indexes:

- default index: `_id`


### Define queries.

- covered query: fields used in the query are part of an index used in the query, the fields
returned in the results are in the same index.

Importance of covered query:

- since all the fields are covered in the index itself, MongoDB can match the query condition
as we'll as return the result fields using the same index without looking inside the documents.
Since indexes are stored in RAM or sequentially located on disk, such access is a lot faster.

Types of queries:

- text-search: 

### Define operators.

- aggregation: processes data records and return computed results. Aggreation operations group
values from multiple documents together, and can perform a variety of operations on the grouped
data to return a signle result. MongoDB provides 3 ways to perform aggregation: the aggregation pipeline, the map-reduce function, and single purpose aggregation methods and commands.


### Define transactions.

### Define lock.

MongoDB uses reader-writer locks that allow concurrent readers shared access to a resource,
such as a database or collection, but give exclusive access to a single write operation.



## Database Admin

### Define replication.

Replication is the process of synchronizing data across multiple servers. Replication provides
redundancy and increases data availability. With multiple copies of data on different database
servers, replication protects a database from the loss of a single server. Replication also
allows you to recover from hardware failure and service interruptions.

- Primary and master nodes are the nodes that can accept writes. MongoDB's replications is
signle-master: only one node can accept write operations at a time.
- Secondary and slave nodes are read-only nodes that replicate from the primary.


### Define sharding.

Sharding is a method for storing data across multiple machines. MongoDB uses sharding to
support deployments with very large data sets and high throughput operations.

### Define GridFS.

GridFS is a specification for storing and retrieving files that exceed the BSON-document
size limit of 16 MB. Instead of storing a file in a single document, GridFS divides a file
into parts, or chunks, and stores each of those chunks as a separate document.


### Define journaling.

When running with journaling, MongoDB stores and applies write operations in memory and
in the on-disk journal before the changes are present in the data files on disk.

Writes to the journal are atomic, ensuring the consistency of the on-disk journal files.
With journaling enabled, MongoDB creates a journal subdirectory within the directory defined by
dbPath, which is `/data/db` by default.



## Database Architecture

### Define storage engine.

A storage engine is the part of a database that is responsible for managing how data is
stored on disk. For example, one storage engine might offer better performance for read-heavy
workloads, and another might support a higher-throughput for write operations.

Types of storage engines:

- WiredTiger: specify the maximum size of the cache that WiredTiger will use for all data. This
can be done using `storage.wiredTiger.engineConfig.cacheSizeGB` option.
- MMAPv1: does not allow configuring the cache size.



### Define profiler.

The database profiler collects fine grained data about MongoDB write operations, cursors,
database commands on a running mongod instance. You can enable profiling on a per-database or
per-instance basis.

The database profiler writes all the data it collects to the system.profile collection, which
is a capped collection.
