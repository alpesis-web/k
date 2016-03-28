---
layout: post_tech
title: "MongoDB Sharding"
date: 2016-03-24 10:59:40 +0800
comments: true
categories: [tech]
tags: [db, mongo]
toc: true
---

## Definitions

Sharding: sharding is a method for storing data across multiple machines.

Purpose of sharding

- high query rate -> CPU capacity of the server
- larger data sets -> storage capacity of a single machine -> RAM stress, I/O capacity of disk drives

Vertical scaling V.S. Sharding

- vertical scaling: adds more CPU and storage resources to increase capacity. Large numbers of CPUs and large amount of RAM, expensive systems
- horizontal scaling (sharding): divides the data set and distributes the data over multiple servers or shards. Each shard is an independent database, and collectively, the shards make up a single logical database. 

Advantages of sharding:

- sharding reduces the number of operations each shard handles. Each shard processes fewer operations as the cluster grows. As a result, a cluster can increase capacity and throughput horizontally. For example, to insert data, the application only needs to access the shard responsible for that record.
- sharding reduces the amount of data that each server needs to store. Each shard stores less data as the cluster grows. For example, if a database has a 1 terabyte data set, and there are 4 shards, then each shard might hold only 256 GB of data. If there are 40 shards, then each shard might hold only 25GB of data.


<img src="https://docs.mongodb.org/manual/_images/sharded-collection.png" />

image source: MongoDB official site

## Concepts

### Shared Clusters

- Shards: store the data. To provide high availability and data consistency, in a production sharded cluster, each shard is a replica set.
- Query Routers (mongos instances): interface with client applications and direct operations to the appropriate shard or shards. A client sends requests to a mongos, which then routes the operations to the shards and returns the results to the clients. A sharded cluster can contain more than one mongos to divide the client request load, and most sharded clusters have more than one mongos for this reason.
- Config Servers: store the cluster's metadata. This data contains a mapping of the clusters's data set to the shards. The query router uses this metadata to target operations to specific shards.

<img src="https://docs.mongodb.org/manual/_images/sharded-cluster.png" />

image source: MongoDB official site

### Shards: Data Partitioning

MongoDB distributes data, or shards, at the collection level. Sharding partitions a collection's data by the shard key.

- Shard key: A shard key is either an indexed field or an indexed compound field that exists in every document in the collection. {key: value} --> chunks (ranged/hash based partitioning)
- ranged based sharding: documents with "close" shard key values are likely to be in the same chuck, and therefore on the same shard. The query router can easily determine which chunks overlap that range and route the query to only those shards that contain these chunks.
- hash based sharding: two documents with "close" shard key values are unlikely to be part of the same chuck. This ensures a more random distribution of a collection in the cluster. Hash key values results in random distribution of data across chunks and therefore shards. But random distribution makes it more likely that a range query on the shard key will not be able to target a few shards but would more likely query every shard in order to return a result. 

<img src="https://docs.mongodb.org/manual/_images/sharding-range-based.png" />

image source: MongoDB official site

<img src="https://docs.mongodb.org/manual/_images/sharding-hash-based.png" />

image source: MongoDB official site

### Config Servers: Balanced Data Distribution


- splitting: splitting is a background process that keeps chunks from growing too large. When a chunk grows beyond a specified chunk size, MongoDB splits the chunk in half. Inserts and updates triggers splits. Splits are an efficient meta-data change. To create splits, MongoDB does not migrate any data or affect eh shards.
- balancer: the balancer is a background process that manages chunk migrations. The balancer can run from any of the mongos instances in a cluster. For example, if collection users has 100 chunks on shard 1 and 50 chunks on shard 2, the balancer will migrate chunks from shard 1 to shard 2 until the collection achieves balance.

<img src="https://docs.mongodb.org/manual/_images/sharding-splitting.png" />

image source: MongoDB official site

<img src="https://docs.mongodb.org/manual/_images/sharding-migrating.png" />

image source: MongoDB official site


### Query Routers: Query Routing

Routing process

- (mongos -> shards)

```
shard key:

{ zipcode: 1, u_id: 1, c_date: 1 }

targeting:

{ zipcode: 1 }
{ zipcode: 1, u_id: 1 }
{ zipcode: 1, u_id: 1, c_date: 1 }

```

detect connection

```
db.isMaster()

{
   "ismaster" : true,
   "msg" : "isdbgrid",
   "maxBsonObjectSize" : 16777216,
   "ok" : 1
}
```

broadcast operations and targeted operations

<img src="https://docs.mongodb.org/manual/_images/sharded-cluster-scatter-gather-query.png" />

image source: MongoDB official site

<img src="https://docs.mongodb.org/manual/_images/sharded-cluster-targeted-query.png" />

image source: MongoDB official site

shared and non-shared data

<img src="https://docs.mongodb.org/manual/_images/sharded-cluster-mixed.png" />

image source: MongoDB official site


## Tutorials

### Deployment

### Maintenance

### Management



## Reference

- [Sharding](https://docs.mongodb.org/manual/core/sharding-introduction/)
