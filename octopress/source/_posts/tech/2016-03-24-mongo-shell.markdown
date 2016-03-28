---
layout: post_tech
title: "Mongo Shell"
date: 2016-03-24 09:54:30 +0800
comments: true
categories: [tech]
tags: [mongo, db]
toc: true
---

## What is Mongo Shell

The Mongo Shell is an interactive Javascript interface to MongoDB.

How to user Mongo Shell:

- query and update data
- perform administrative operations

### Start Shell

```
$ mongo
> show dbs
> use <dbname>
> db.<mycollection>.insert({<key>: <value>});
> db[<mycollection>].find();
> db.getCollection(<mycollection>).find();
> db.<mycollection>.find().pretty();
> quit()
```

## Customizing Mongo Shell

- config file: ` .mongorc.js`

display # of operations

```javascript
cmdCount = 1;
prompt = function() {
             return (cmdCount++) + "> ";
         }

1>
2>
3>
```

display database and hostname

```javascript
host = db.serverStatus().host;

prompt = function() {
             return db+"@"+host+"$ ";
         }

test@myHost1$
```

display up time and document count

```javascript
prompt = function() {
           return "Uptime:"+db.serverStatus().uptime+" Documents:"+db.stats().objects+" > ";
         }

Uptime:5897 Documents:6 >
```

editor and batch size

```bash
# editor
export EDITOR=vim
mongo

# batch size
DBQuery.shellBatchSize = 10;
```

## Shell Help

- command line
- shell
- database

```bash
# command line
> mongo --help

# shell
> help
```

database help

```bash
# mongo
> show dbs
> db.help()
> db.updateUser


# collections
> show collections
> db.collection.help()
> db.collection.save

# cursor
> db.collection.find().help()
> db.collection.find().toArray

# wrapper object
> help misc
```

## Scripting

sripts: `*.js`

```javascript
new Mongo()
new Mongo(<host>)
new Mongo(<host:port>)

conn = new Mongo();
db = conn.getDB("myDatabase");

db = connect("localhost:27020/myDatabase");

cursor = db.collection.find();
while ( cursor.hasNext() ) {
   printjson( cursor.next() );
}
```

how to run

```bash
> mongo test --eval "printjson(db.getCollectionNames())"
> mongo localhost:27017/test myjsfile.js
> load("myjstest.js")
> load("scripts/myjstest.js")
> load("/data/db/scripts/myjstest.js")
```

## Data Types

Date

- Date() method which returns the current date as a string.
- new Date() constructor which returns a Date object using the ISODate() wrapper.
- ISODate() constructor which returns a Date object using the ISODate() wrapper.

```javascript
> var myDateString = Date();
> myDateString
Wed Dec 19 2012 01:03:25 GMT-0500 (EST)
> typeof myDateString

> var myDate = new Date();
> var myDateInitUsingISODateWrapper = ISODate();
> myDate
ISODate("2012-12-19T06:01:17.171Z")
> myDate instanceof Date
> myDateInitUsingISODateWrapper instanceof Date
```

ObjectId

```javascript
new ObjectId
```

NumberLong

```javascript
NumberLong("2090845886852")
db.collection.insert( { _id: 10, calc: NumberLong("2090845886852") } )
db.collection.update( { _id: 10 },
                      { $set:  { calc: NumberLong("2555555000000") } } )
db.collection.update( { _id: 10 },
                      { $inc: { calc: NumberLong(5) } } )

db.collection.findOne( { _id: 10 } )
{ "_id" : 10, "calc" : NumberLong("2555555000005") }
```

typeof

```javascript
mydoc._id instanceof ObjectId
typeof mydoc._id
```

## Reference

- [The Mongo Shell](https://docs.mongodb.org/manual/mongo/)
