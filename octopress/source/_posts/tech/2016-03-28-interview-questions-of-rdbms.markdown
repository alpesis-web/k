---
layout: post_tech
title: "Interview Questions of RDBMS"
date: 2016-03-28 16:49:53 +0800
comments: true
categories: [tech]
tags: [db, rdbms]
toc: true
---

## Concepts

### Define database.

### Define data warehousing.

Storage and access of data from the central location in order to take some strategic decision is
called data warehousing. Enterprise management is used for managing the information whose framework is known as Data Warehousing.


## Database Design

### Database Modeling.

- entities -> relationships -> ERM
- attributes -> types
- normalization

### Enlist the various relationships of database.

- one-to-one: single table having drawn relationship with another table having similar kind of columns.
- one-to-many: two tables having primary and foreign key relation.
- many-to-many: junction table having many tables relatd to many tables.

### Define normalization.

Organized data void of inconsistent dependency and redundancy within a database is called normalization.

- 1NF (attribute):
- 2NF (record):
- 3NF (table):

Advantages of normalization:

- non duplicate entries
- saves storage space
- boasts the query performances

### Define denormalization.

Boosting up database performance, adding of redundant data which in turn helps rid of complex data is called denormalization.


## Database Languages

### Define DDL.

Managing properties and attributes of database is called data definition language (DDL).

- CREATE: create is used in `CREATE TABLE [column name] ([ column definitions ]) [table parameters]`
- ALTER: it helps in modification of an existing object of database.
- DROP: it destroys an exisiting database, index, table or view.

```sql
CREATE TABLE [column name] ([column definitions]) [table parameters]
ALTER objecttype objectname parameters
DROP objecttype objectname
```


### Define DML.

Manipulating data in a database such as inserting, updating, deleting is defined as Data Manipulation Language (DML).


### Define cursor.

A database object which helps in manipulating data row by row representing a result set is called cursor.

cursor types:

- dynamic: it relects changes while scrolling.
- static: doesn't reflect changes while scrolling and works on recording of snapshot.
- keyset: data modification without reflection of new data is seen.

types of cursor:

- implicit cursor: declared automatically as soon as the execution of SQL takes place without the wareness of the user.
- explicit cursor: defined by PL/SQL which handles query in more than one row.


### Define operators.

- union all: full recordings of two tables
- union: a distinct recording of two tables

- group: uses aggregrate values to be derived by collecting similar data.

- aggregation: operates against a collection of values and returning single value

- join: helps in explaining the relation between different tables. They also enable you to select data with relation to data in another table.
    - inner joins: blank rows are left in the middle while more than equal to two tables are joined.
    - outer joins: devided into left outer join and right outer join. Blank rows are left at the specified side by joining tables in other side.
    - ohter joins: cross joins, natural joins, equi join, non-equi join. 



### Define indexes.

- non-clustered index: B-tree structure, has data pointers enabling one table many non-clustered indexes.
- clustered index: B-tree structure, distinct for every table.


Index hunting: Indexes help in improving the speed as well as the query performance of database.
The procedure of boosting the collection of indexes is named as Index hunting.

- the query optimizer is used to coordinate the study of queries with the workload and the best
use of queries suggested based on this.
- Index, query distribution along with their performance is observed to check the effect.
Tuning databases to a small collection of problem queries is also recommended.


### Define view.


Restrictions of views:

- only the current database can have views.
- you have not liable to change any computed value in any particular view.
- integrity constants decide the functionality of INSERT and DELETE.
- temporary views cannot be created.
- temporary tables cannot contain views.
- non association with DEFAULT definitions.
- triggers such as INSTEAD OF is associated with views.


### Define transactions.

Transaction phases:

- analysis phase
- redo phase
- undo phase

ACID:

- Atomicity
- Consistence
- Isolation
- Durability


## Database Admin

### Define partitioning.

Database partitioning: division of logical database into independent complete units for
improving its management, availability and performance. Splitting of one table which is
large into smaller database entities logically.

The importance of partitioning:

- to improve query performance in situations dramatically when mostly rows which are heavily
accessed are in one partition.
- accessing large parts of a single partition.
- slower and cheaper storage media can be used for data which is seldom used. 
