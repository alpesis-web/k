---
layout: post_tech
title: "MongoDB CRUD"
date: 2016-03-24 10:49:19 +0800
comments: true
categories: [tech]
tags: [db, mongo]
toc: true
---

## Definitions

- BSON: BSON is a binary representation of JSON with additional type information.
- Document (record): a record with {key, value}
- Collection (table): a collection is a group of related documents that have a set of shared common indexes.


<img src="https://docs.mongodb.org/manual/_images/crud-query-stages.png" />

image source: MongoDB official site

<img src="https://docs.mongodb.org/manual/_images/crud-insert-stages.png" />

image source: MongoDB official site

## Concepts

### Read

- query interface
- query behavior
- query statement
- query projection
- cursors

<img src="https://docs.mongodb.org/manual/_images/crud-annotated-mongodb-find.png" />

image source: MongoDB official site

```javascript
db.users.find( { age: { $gt: 18 } }, { name: 1, address: 1 } ).limit(5)
```

<img src="https://docs.mongodb.org/manual/_images/crud-query-stages.png" />

image source: MongoDB official site


Projection

<img src="https://docs.mongodb.org/manual/_images/crud-query-w-projection-stages.png" />

image source: MongoDB official site

```javascript
db.records.find( { "user_id": { $lt: 42 } }, { "history": 0 } )
db.records.find( { "user_id": { $lt: 42 } }, { "name": 1, "email": 1 } )
db.records.find( { "user_id": { $lt: 42} }, { "_id": 0, "name": 1 , "email": 1 } )
```

### Write

## Tutorials


## Reference

- [MongoDB CRUD Operations](https://docs.mongodb.org/manual/core/crud-introduction/)
