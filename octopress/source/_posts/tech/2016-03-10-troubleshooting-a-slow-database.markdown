---
layout: post_tech
title: "Troubleshooting A Slow Database"
date: 2016-03-10 13:55:12 +0800
comments: true
categories: [tech]
tags: [db]
toc: true
---

## SQL Server

The causes for performance problems can be various, but the most common are a poorly designed databse, incorrectly configured system, insufficient disk space or other system resources, excessive query complication and recomplication, bad execution plans due to missing or outdated statistics, and queries or stored procedures that have long execution times due to improper design.

The most common SQL Server performance symptoms are

- CPU
- Memory
- Network
- I/O
- Slow running queries

### CPU

CPU bottlenecks are cuased by insufficient hardware resources. Troubleshooting starts with identifying the biggest CPU resource users. Occasional peaks in processor usage can be ignored, but if the processor is constantly under pressure, investigation is needed. Adding additional processors or using a more powerful one might not fix the problem, as badly designed processes can always use all CPU time. Query tuning, improving execution plans, and system reconfiguration can help. To avoid bottlenecks, it's recommended to have a dedicated server that will run only SQL Server, and to remove all other software to another machine.

To start troubleshooting the most common CPU performance issues, monitor % Processor Time. This counter is available in Performance Monitor. If its value is constantly higher than 80%, the processor is under pressure.

The counters that indicate most common causes for processor pressure are Batch Requests/sec, SQL Complications/sec, and SQL Recomplications/sec. These counters are available in Performance Monitor and in the `sys.dm_os_performance_counters` view.

```sql
select * from sys.dm_os_performance_counters
where counter_name in ('Batch Requests/sec', 'SQL Complications/sec', 'SQL Re-Compilations/sec')

///////////////////////////////////////////

DECLARE @BatchRequests BIGINT;

SELECT @BatchRequests = cntr_value
FROM sys.dm_os_performance_counters
WHERE counter_name = 'Batch Requests/sec';

WAITFOR DELAY '00:00:10';

SELECT (cntr_value - @BatchRequests) / 10 AS 'Batch Requests/sec'
FROM sys.dm_os_performance_counters
WHERE counter_name = 'Batch Requests/sec';
```

Evaluations

- Batch Requests/sec value depends on hardware used, but it should be under 1000
- SQL Complications/sec is less than 10% of Batch Requests/sec
- SQL Re-Complications/sec is less than 10% of SQL Complications/sec


### Memory

Memory bottlenecks can result in slow application responsiveness, overall system slowdown, or even application crashing. It's recommended to identify when the system runs with insufficient memory, what applications use most of memory resources, whether there are bottlenecks for other system resources. Reviewing and tuning queries, memory reconfiguration, and adding more physical memory can help.

Memory bottlenecks are caused by limitations in available memory and memory pressure caused by SQL Server, system, or other application activity. Poor indexing requires table scans which in case of large tables means that a large number of rows is read from disk and handled in memory.

For memory problems, monitor the Memory Available KB performance counter. The normal values should be over 200 MB. If the value of Memory Available KB counter is lower than 100 MB for long time, it's a clear indication of insufficient memory on the server. This counter is available in Performance Monitor, and two more useful counters - Total Server Memory (KB) and Target Server Memory (KB) are available via the `sys.dm_os_performance_counters` view.

Another counter to monitor is Pages/sec, it is available in Performance Monitor. It shows the rate at which the pages are written from disk to RAM and read froom RAM to disk. The values higher than 50 show intensive memory activity and possible overhead and memory pressure that can lead to SQL Server performance degradation.

Checkpoint pages/sec and Lazy writes/sec indicate whether dirty pages are flushed to disk too often. Dirty pages are automatically flushed to disk at a checkpoint. If the available free space in the buffer cache between two checkpoints is low, a lazy write will occur to flush the pages from buffer to disk and free up memory. The Lazy Writes/sec value should be below 20. Both counters are available in Performance Monitor and the `sys.dm_os_performance_counters` view.

If the Lazy Writes/sec value is constantly above the threshold, check the Page Life Expectancy value. Values below 300 seconds indicate memory pressure. The counter is available in Performance Monitor and the `sys.dm_os_performance_counters` view.

Buffer Cache Hit Ratio shows the ratio of the data pages found and read from the SQL Server buffer cache and all data page requests.. If a page doesn't exist in the buffer cache, it has to be read disk, which downgrades performance. The recommended value is over 90. The counter is available in Perforamnce Monitor and the `sys.dm_os_performance_counter` view.

```
Buffer Cache Hit Ratio % = 100 * Buffer Cache Hit Ratio / Buffer Cache Hit Ratio Base
                         = 100 * 1797 / 1975
                         = 90.98%
```

Evaluations

- Memory Available KB > 200 MB
- Total Server Memory
- Target Server Memory
- Pages/sec < 50
- Writes/sec < 20
- Page Life Expectancy > 300 seconds
- Buffer Cache Hit Ratio > 90

### Network

Network bottlenecks might not be instantly recognized, as they can at a first glance be considered as SQL Server performance issues caused by other resources. For example, a delay of data sent over a network can look like SQL Server slow response.

Network bottlenecks are caused by overload on a server or network, so the data cannot flow as expected.

Troubleshooting network problems should start with finding queries, functions, and stored procedures that have slow response time. If they are executed quickly, but with a large delay between two calls, it can be an indication of a network issue. SQL Server Profiler can be used to determine which queries, functions and stored procedures were executed.


### I/O

I/O bottlenecks are caused by excessive reading and writing of database pages from and onto disk. A bottleneck is manifested through long response times, application slowdowns and tasks time-outs. If other applications use disk resources excessively, SQL Server might not get enogh disk reources for its normal operation and would have to wait to be able to read and write to disk.

I/O issues can be caused by slow hardware used, bad storage solution design, and configuration. Besides hardware components, such as disk types, disk array type, and RAID configuration that affect I/O performance, unnecessary requests made by a database also affect I/O traffic. Frequent index scans, inefficient queries, and out of date statistics can also cause I/O workload and bottlenecks.

For I/O problems, monitor disk-related counters: Average Disk Queue Length, Average Disk Sec/Read, Average Disk Sec/Write, % Disk Time, Average Disk Reads/Sec, and Average Disk Writes/Sec. All counters are available in Performance Monitor.

Average Disk Queue Length shows the average number of I/O operations that are waiting to be written to or read from disk and the number of currently processed reads and writes. The recommended value is below 2 per individual disk, and higher values indicate I/O bottlenecks.

Average Disk Sec/Read shows the average time in seconds needed to read data from disk. The recommended values are given for categories, where under 8ms is excellent performance, and higher than 20ms is a serious I/O issue.

Average Disk Sec/Write shows the average time in seconds needed to write data to disk. The performance is excellent if the value is below 1ms and bad if the counter value is higher than 4ms.

Average Disk Reads/Sec and Average Disk Writes/Sec show the rate of read and write operations on disk, respectively. Low values indicate slow disk I/O processing, and checking processor usage and disk-expensive queries is recommended. The normal values depend on disk specification and server configuration. These counters don't have a specific threshold, so it's recommended to monitor these metrics for a while and to determine treads and set a baseline.


Evaluations

- Average Disk Queue Length < 2 per individual disk
- Average Disk Sec/Read < 8ms (if > 20ms -> serious I/O issue)
- Average Disk Sec/Write < 1ms (if > 4ms -> serious I/O issue)
- % Disk Time
- Average Disk Reads/Sec
- Average Disk Writes/Sec

### Slow running queries

SQL Server performance is affected by many factors, the most common SQL Server performance killer are 

- poor database design
- poor indexing
- poor query design
- not reusable execution plans
- frequent query recompilation
- excessive fragmentation 

Slow running queries can be result of missing indexes, poor execution plans, bad application and schema design, etc.

An index is used to speed up data search and SQL query performance. The database indexes reduce the number of data pages that have to be read in order to find the specific record.

A table without a clustered index is called a heap, due to its unordered structure. Data in a heap table isn't sorted, usually the records are added one after another, as they are inserted into the table. They can also be rearranged by the database engine, but again, without a specific order. When you insert a lot of rows into a heap table, the new records are written on data pages without a specific order. Finding a record in a heap table can be compared to finding a specific leaf in a heap of leaves. It is inefficient and requires time.

A heap can have one or several nonclustered indexes, or no indexes at all.

A nonclustered index is made only of index pages that contain row locators (pointers) for records in data pages. It doesn't contain data pages, like clustered indexes.

A clustered index organizes table data, so data is queried quicker and more efficiently. A clustered index conosists of both index and data pages, while a heap table has no index pages; it consists only of data pages. In other words, it is not just an index, i.e. a pointer to the data row that contains the key value, but it also contains table data. The data in the clustered table is sorted using the values of the columns the clustered index is made of.

Finding a record from a table with a proper clustered index is quick and easy like finding a name in an alphabetically ordered list. A general recommendation for all SQL tables is to have a proper clustered index.

While there can be only one clustered index on a table, a table can have up to 999 nonclustered indexes.

Indexes can be created using T-SQL.


```sql clustered index
CREATE TABLE [Person].[Address](
    [AddressID] [int] IDENTITY(1,1) NOT FOR REPLICATION NOT NULL,
    [AddressLine1] [nvarchar](60) NOT NULL,
    [AddressLine2] [nvarchar](60) NULL,
    [City] [nvarchar](30) NOT NULL)


CONSTRAINT [PK_Address_AddressID] PRIMARY KEY CLUSTERED(
    [AddressID] ASC
) ON [PRIMARY]
```

When you execute the select statement on a clustered table where an ascending clustered index
is created, the results will be ordered ascending by the clustered key column. In this example,
it's the AddressID column.

```sql nonclustered index
CREATE TABLE [Person].[Address4](
    [AddressID] [int] IDENTITY(1,1) NOT FOR REPLICATION NOT NULL,
    [AddressLine1] [nvarchar](60) NOT NULL,
    [AddressLine2] [nvarchar](60) NULL,
    [City] [nvarchar](30) NOT NULL
)

CREATE NONCLUSTERED INDEX [IX_Address_StateProvinceID4] ON [Person].[Address4](
    [AddressID] ASC
) ON [PRIMARY]
```

When searching for a specific record in a heap table, SQL Server has to go through all table
rows. This can be acceptable on a table with a small number of records.

As rows in a heap table are unordered, they are identified by a row identifier (RID) which
consists of the file number, page number, and slot number of the row.

It's not recommended to use a heap table if you're going to store a large number of records
in the table. SQL query execution on a table with millions of records without a clustered
index requires a lot of time. Also, if you need to get a sorted results list, it's easier
to define an ascending or descending clustered index, as shown in the examples above, than
to sort the unsorted reuslts set retrieved from a heap table. The same goes for grouping,
filtering by a value range.

Indexing strategy is complex; it depends on many factors, including 

- database structure
- queries
- stored procedures used

One of general recommendations is to create a clustered index on tables where data is frequently
queried. Although some DBAs and developers don't prefer having clustered indexes on tables frequently inserted or updated, others consider that a clustered index on the right column can improve
performance in these situations.

Creating a clustered index on every table is highly recommended, the challenge is to create
the right index. With a proper clustered index, less reads are required to retrieve the
records the records requested by a query or stored procedure. Therefore, fewer disk I/O
are performed and the operation is completed faster.

A clustered index provides more efficient search for values in a specific range. When you
already have a table where the records are sorted ascending by e.g. AddressID, it's easy
to find the rows where AddressID is between 100 and 200, or lower than 500.

One of the rare scenarios where a heap table can be a good practice is when the row
identifier is smaller than the clustered index.

Poor Indexing

Any SQL Server table configuration where performance suffers due to excessive, improper,
or missing indexes is considered to be poor indexing.

If indexes are not properly created, SQL Server has to go through more records in order
to retrieve the data requested by a query. Therefore, it uses more hardware resources
(proessor, memory, disk, and network) and obtaining the data lasts longer.

A wrong index can be an index created on a column that doesn't provide easier data
manipulation or an index created on multiple columns which instead of speeding up
queries, slows them down.

A table without a clustered index can also be considered as a poor indexing practice.
Execution of a SELECT statement, inserting, updating and deleting records is in most
cases slower on a heap table than on a clustered one.

Which columns to use to build an index?

Both clustered and nonclustered indexes can be built from one or more table columns.

When you create a new table with a primary key in a SQL Server database, a unique clustered
index is automatically created on the primary key column. Although this default action is
acceptable in most cases, this might not be the optimal clustered index.

The column used for a clustered index should be a unique, identity, or primary key, or
any other column where the value is increased for each new entry. As clustered indexes
sort the records based on the value, using a column already ordered ascending, such
as an identity column, is a good solution.

If a column where new values are not higher than previous is used for a clustered index,
adding each new row would require re-ordering, i.e. moving the whole row and placing it
to its proper location in accordance with clustered index ordering, thus splitting data
pages and affecting SQL Server performance. If such clustered index is created on a table
with frequent inserts and updates, it can cause performance degradation.

It's not recommended to use the primary key as a clustered key without checking whether
that is the optimal solution in your scenario first. Also, note the difference between
a primary key and clustered index - a primary key can't have duplicate or null values,
while a clustered index can.

Using a unique column for a clustered index enables more efficient search for a specifc
value. On the other hand, a column that frequently changes its value should not used
for a clustered index. Each change of of the column used for the clustered index
requires the records to be reordered. This re-ordering can easily be avoided by using
a column that is not updated frequently, or not updated at all.

Using a column that stores large data, such as BLOB columns (text, nvarchar(max), image, etc.),
and GUID columns is not recommended. Using large values to sort the data is not efficient,
and in case of GUID and image columns doesn't seem to make sense.

A clustered index should not be built on a column already used in a unique index.

```sql
CREATE TABLE [Person].[Address](
    [AddressID] [int] IDENTITY(1, 1) NOT FOR REPLICATION NOT NULL,
    [AddressLine1] [nvarchar](60) NOT NULL,
    [AddressLine2] [nvarchar](60) NULL,
    [City] [nvarchar](30) NOT NULL,
    [StateProvinceID] [int] NOT NULL,
    [PostalCode] [nvarchar](15) NOT NULL,
    [SpatialLocation] [geography] NULL,
    [rowguid] [uniqueidentifier] ROWGUIDCOL NOT BULL,
    [ModifiedDate] [datetime] NOT NULL,
    CONSTRAINT [PK_Address_AddressID] PRIMARY KEY CLUSTERED ([AddressID ASC] WITH(
        PAD_INDEX = OFF,
        STATISTICS_NORECOMPUTE = OFF,
        IGNORE_DUP_KEY = OFF,
        ALLOW_ROW_LOCKS = ON,
        ALLOW_PAGE_LOCKS = ON
        ) ON [PRIMARY]
    ) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
)
GO


CREATE CLUSTERED INDEX [Clustered_City_Address1] ON [Person].[Address1](
    [City] ASC
) WITH (
        PAD_INDEX = OFF,
        STATISTICS_NORECOMPUTE = OFF,
        SORT_IN_TEMPDB = OFF,
        DROP_EXISTING = OFF,
        ONLINE = OFF,
        ALLOW_ROW_LOCKS = ON,
        ALLOW_PAGE_LOCKS = ON
    ) ON [PRIMARY]
GO
```

Disadvantages of using indexes

As noted above, wrong indexes can significantly slow down SQL Server Performance.
But even the indexes that provide better performance for some operations, can
add overhead for others.

While executing a SELECT statement is faster on a clustered table, INSERTs, UPDATEs,
and DELETEs require more time, as not only data is updated, but the indexes are updated also.
For clustered indexes, the time increase is more significant, as the records have to maintain
the correct order in data pages. Whether a new record is inserted, or an existing deleted or
updated, this usually requires the records to be reordered.

Another cost of having indexes on tables is that more data pages and memory is used.

The operator costs from the Actual Query Execution Plan for the tables mentioned above
are shown below. The table with the clustered index on the primary key column is
[Person].[Address], the table with the clustered index on a non-primary key column
is [Person].[Address1]. We created two more copies of the the same table, one with
a nonclustered index on the primary key column and the other without any indexes.
The data in all four tables is identical.

When it comes to executing a SELECT statement, the cost is the lowest when the list
of returned columns is specified and the statement is executed on the table where
the clustered index is created on the primary key column. The cost can be higher
for a table with a non-optimal clustered index, then on tables with a nonclustered index
or no indexes at all.

While executing a SELECT statement is faster on a clustered table, executing DELETEs
and UPDATEs requires more time. For the latter statements, the performance of a table
with a nonclustered index is the same as for the table with a clustered index on a 
column other than the primary key.

Note that INSERTs on a table without indexes is the fastest of all - this is expected
as neither re-ordering nor index updating is required. On the same table, executing
UPDATEs and DELETEs is the most expensive. Again, this is expected, as SQL Server
requires most time to find the specific records in such table.

As shown, indexes can speed up some queries and slow down others. 


#### queries

- T-SQL
- DML: SELECT, INSERT, DELETE, UPDATE
- conditional statements: IF...THEN...ELSE
- relational operators: table scans, aggregations, joins
- declare cursor


T-SQL statements and stored procedures are presented as tree roots. Statements called
by the stored procedure are presented as children in the tree.

```sql
EXEC sp_who2 243
```

Data manipulation language (DML) statements SELECT, INSERT, DELETE, and UPDATE are
also presented as tree roots. The first child represents the execution plan for
the statement. If the statement fires a trigger, it's represented as the second child.

```sql
DELETE FROM [dbo].[AUDIT_LOG_TRANSACTIONS]
WHERE [AUDIT_LOG_TRANSACTION_ID] = 1
```

The conditional statements, such as `IF...THEN...ELSE` are presented with 3 children.
WHILE and DO-UNTIL statements are represented similarly.

```sql
IF
    (SELECT COUNT(*) FROM [Person].[Address] WHERE City LIKE 'Bothell') > 0
    (SELECT COUNT(*) FROM [Person].[Address] WHERE City LIKE 'Bothell')
Else
    (SELECT COUNT(*) FROM [Person].[Address] WHERE AddressLine2 is null)
```

Relational operators, such as table scans, aggregations, and joins are presented in the tree
as nodes.

```sql
SELECT PrV.Standardprice, V.Name, V.CreditRating
FROM Purchasing.ProductVendor AS PrV
JOIN Purchasing.Vendor AS V
ON (PrV.BusinessEntityID = V.BusinessEntityID)
```

The DECLARE CURSOR statement is shown as the tree root. The statement it refers to is
shown as a child.

```sql
DECLARE vend_cursor CURSOR
FOR SELECT * FROM Purchasing.Vendor
OPEN vend_cursor
FETCH NEXT FROM vend_cursor;
```


## References

- [DBA Guide: SQL Server Performance Troubleshooting](http://www.sqlshack.com/dba-guide-sql-server-performance-troubleshooting-part-1-problems-performance-metrics/)
