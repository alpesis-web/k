---
layout: post_tech
title: "Databases"
date: 2016-03-13 21:23:22 +0800
comments: true
categories: [tech]
tags: [db]
toc: true
---

## Database Overviews

### What is Database

Database - any collection of related data, a persistent, logically coherent collection of inherently meaningful data, relevant to some aspects of the real world.

Database Management System (DBMS) - a collection of programs that enables useres to create and maintain a database.

Relational Database Management System (RDBMS) - a database that treats all of its data as a collection of relations:

- SET: A = {1,2,3,4}
- Cartesian Product: B * C = {<H,R>, <H,B>, <T,R>, <T,B>}
- Relation: Q = {<H,R>, <H,B>}

Non-Relational Database Management System (NoRDBMS) - xxx

### Types of Databases

#### SQL

#### NoSQL

General

- **Key-Value/Ordered Key-Value**: the simplest NoSQL databases, every single item in the database is stored as an attribute name or key, together with its value.
- **BigTable (wide-column)**: optimized for queries over large datasetes, and store columns of data together, instead of rows.
- **Document, Full-Text Search**: pair each key with a complex data structure known as a document, documents can contain many diffferent key-value pairs, or key-array pairs, or even nested documents.
- **Graph**: used to store information about networks, such as social connections.


Others

- Multimodel
- Object
- Grid & Cloud
- XML
- Multidimensional
- Network Model



| Type                       | Databases                                               |
|:---------------------------|:--------------------------------------------------------|
| Key-Value/Ordered Key-Value| Amazon DynamoDB, Azure Table storage, Riak, Redis, Oracle NoSQL Database, MemcacheDB, PickleDB, Aerospike, FoundationDB, Berkeley DB, GenieDB, BangDB, Scalaris, Tokyo Cabnit/Tyrant, Voldemort, Dynomite, c-treeACE database, KitaroDB, hamsterdb, STSdb, Tarantool, quasardb, RaptorDB, TIBCO ActiveSpaces DB, NessDB, HyperDex, Symas Lightning Memory Mapped Database (LMDB), Light Cloud, Hibari, Genome |
| BigTable (column-based)    | Hadoop/HBase, Cassandra, Hypertable, Accumulo, Amazon SimpleDB, Cloud Data, HPCC, Flink, Splice |
| Document, Full-Text Search | MongoDB, Elastic Search, Couchbase Server, CouchDB, RethinkDB, RavenDB, MarkLogic Server, Clusterpoint Server, NeDB, Terrastore, JasDB, RaptorDB, Djondb, EDB, Amisa Server, DensoDB, SisoDB, SDB, UnQLite, ThruDB |
| Graph                       | Neo4J, InfiniteGraph, DEX, Titan, Infogrid, HypergraphDB, Trinity, AllegroGraph, WHITE Database, Virtuoso, VertxDB, FlockDB, BrightstarDB |
| Multimodel                 | ArangoDB, OrientDB, DatomicDB, FatDB, AlchemyDB, coretxDB |
| Object                     | VersantDB, Objectivity, Gemstone, NinjaDB, ObjectDB, Starcounter, HSS Database, ZODB, Magma, NEODB, Streling, EyeDB, FarmerD |
| Grid & Cloud               | Oracle Coherence, GemfireDB, Infinispan, Hazelcast |
| XML                        | EMC Documentum xDB, eXist, Sedna, BaseX, Qizx/db, BerkeleyDB |
| Multidimensional           | Global, Intersystem cache, GT.M, SciDB, Rasdaman |
| Network Model              | Vyhodb |



### Database Systems


```
(fontend) SQL user interface     forms interface     report generation tools     data mining & analysis
--------------------------------------------------------------------------------------------------------
(SQL API) interface
--------------------------------------------------------------------------------------------------------
(backend) SQL Engine
```


## Frontend: User Level

### Database Design

steps:

- entities --> relationships (1..1, 1..n, n..1, n..m) --> ERM (entity-relationship model)
- attributes --> keys --> data type
- normalization (normal forms: 1NF, 2NF, 3NF)


- identifying entities: (types of entities) people, things, events, locations.
- identifying relationships: 1:1, 1:N, M:1, and M:N
- normalization: 
  - 1NF: there may be no repeating grouops of columns in an entity.
  - 2NF: all attributes of an entity shoudl be fully dependent on the whole primary key.
  - 3NF: all sttributes need to be directly dependent on the primary key, and not on other attributes. 2NF you point out attributes through the PK, in 3NF every attribute needs to be dependent on the PK, and nothing else.



### Database Languages

- Data Languages
  - DDL (Data Definition Language): create/drop/alter/rename table
  - DML (Data Manipulation Langauge): CRUD - select, insert, update, delete
  - DQL (Data Query Language): select, show, help
  - DCL (Data Control Language): grant, revoke
  - DTL (Data Transaction Language): ACID - start transaction, savepoint, commit, rollback
- VDL (View Definition Language): user views
- SDL (Storage Definition Language): internal schemas


#### Data Languages

##### DDL

```sql
CREATE TABLE [table name] ( [column definitions] ) [table parameters]
CREATE TABLE employees (
    id            INTEGER      PRIMARY KEY,
    first_name    VARCHAR(50) not null,
    last_name     VARCHAR(75) not null,
    fname         VARCHAR(50) not null,
    dateofbirth   DATE         not null
);

DROP objecttype objectname
DROP TABLE employees;

ALTER objecttype objectname parameters
ALTER TABLE sink ADD bubbles INTEGER;
ALTER TABLE sink DROP COLUMN bubbles;

RENAME TABLE old_name TO new_name;
```


##### DML

CRUD: Create, Read, Update, Delete

```
Operation          SQL      HTTP              DDS
Create             INSERT   PUT/POST          write
Read (Retrieve)    SELECT   GET               read/take
Update (Modify)    UPDATE   POST/PUT/PATCH    write
Delete (Destroy)   DELETE   DELETE            dispose
```

Syntax

```sql
SELECT ... FROM ... WHERE ...
INSERT INTO ... VALUES ...
UPDATE ... SET ... WHERE ...
DELETE FROM ... WHERE ...

SELECT nom, service
FROM employe
WHERE status = 'stagiaire'
ORDER BY nom;

INSERT INTO employees(first_name, last_name, fname) VALUES ('John', 'Capita', 'xcapit00');
UPDATE a_table SET field1 = 'updated value' WHERE field2 = 'N';
DELETE FROM a_table WHERE field2 = 'N';

MERGE INTO table_name USING table_reference ON (condition)
   WHEN MATCHED THEN
   UPDATE SET column1 = value1 [, column2 = value2 ...]
   WHEN NOT MATCHED THEN
   INSERT (column1 [, column2 ...]) VALUES (value1 [, value2 ...
```

##### DQL

```sql
- functions: COUNT, AVG, MIN, MAX, SUM

SELECT *    column(s)
FROM *
            table(s)
            FULL OUTER JOIN
            RIGHT OUTER JOIN
            LEFT OUTER JOIN
            INNER JOIN
            Alias (AS)
WHERE *
            Predicate (=,<>,<,<=,>,>=)
            Operators (AND, OR, NOT)
            LIKE (%,_)
            BETWEEN ... IN ...
            EXISTS
            IS NULL
ORDER BY *
            ASC/DESC
GROUP BY *
            column(s)
HAVING *
            clause
FETCH *
            clause
```


##### DCL

```sql
GRANT SELECT, UPDATE
ON example
TO some_user, another_user;

REVOKE SELECT, UPDATE
ON example
FROM some_user, another_user;
```

##### DTL 

ACID: Atomicity, Consistency, Isolation, Durability

```sql
commit;
savepoint;
rollback;
```

#### VDL

```sql
CREATE VIEW "VIEW_NAME" AS "SQL Statement";

CREATE VIEW V_Customer
AS SELECT First_Name, Last_Name, Country
FROM Customer;

CREATE VIEW V_REGION_SALES
AS SELECT A1.Region_Name REGION, SUM(A2.Sales) SALES
FROM Geography A1, Store_Information A2
WHERE A1.Store_Name = A2.Store_Name
GROUP BY A1.Region_Name;

SELECT * FROM V_REGION_SALES;
```

#### SDL

```sql

```

### Database Management

#### Best Practice

#### Performance

#### Scalability 

#### Troubleshooting

The causes for performance problems can be various, but the most common are a poorly designed databse, incorrectly configured system, insufficient disk space or other system resources, excessive query complication and recomplication, bad execution plans due to missing or outdated statistics, and queries or stored procedures that have long execution times due to improper design.

The most common SQL Server performance symptoms are

- CPU
- Memory
- Network
- I/O
- Slow running queries


### Database Users

- designer: database design
- Developer: write programs to CRUB from/to database
- DBA: admin database of information, users, security/integrity, backup/recovery, performance
- Analyst: Modeling
- Users: query and manipulation


## Middleware: API Level

### Types of API

## Backend: Database Level

### Database Architecture

## References

### SQL

- [Database Fundamentals](http://www.esp.org/db-fund.pdf)
- [Architecture of A Database System](http://db.cs.berkeley.edu/papers/fntdb07-architecture.pdf)
- [Database System Architectures](http://codex.cs.yale.edu/avi/db-book/db6/slide-dir/PDF-dir/ch17.pdf)

### NoSQL

databases:

- [A Deep Dive Into NoSQL: A Complete List of NoSQL Databases](http://bigdata-madesimple.com/a-deep-dive-into-nosql-a-complete-list-of-nosql-databases/)


### Database Design and Modeling

- [Introuction DB Modeling](http://www.datanamic.com/support/lt-dez005-introduction-db-modeling.html)
- [NoSQL Data Modeling Techniques](https://highlyscalable.wordpress.com/2012/03/01/nosql-data-modeling-techniques/)
