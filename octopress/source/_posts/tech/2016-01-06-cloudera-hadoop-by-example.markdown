---
layout: post_tech
title: "Cloudera Hadoop By Example"
date: 2016-01-06 22:05:08 +0800
comments: true
categories: [tech]
tags: [cluster, cloudera, hadoop, sqoop, hive, impala, beeline, spark, solr, flume]
toc: true
---


Contents

- analyzing structured data (sqoop, impala)
- analyzing unstructured data (beeline, impala)
- analyzing data in realtime (spark)
- indexing and searching data (solr)

## Analyzing Structured Data: MySQL Data

Tools:

- sqoop
- hive
- hdfs
- avro
- mysql
- mapreduce


### Loading data by sqoop

Steps:

- connecting MySQL database
- lauching MapReduce jobs
- putting the export files in Avro format in HDFS, and creating the Avro schema
- (later) loading Hive tables for use in Impala

```bash loading data from local to HDFS by sqoop
$ cd /path/to/project

$ sqoop import-all-tables \
-m 1 \
--connect jdbc:mysql://quickstart:3306/retail_db \
--username=retail_dba \
--password=cloudera \
--compression-codec=snappy \
--as-avrodatafile \
--warehouse-dir=/user/hive/warehouse
```

### Validation

NOTE: `*.avsc` files is located on the local system.

```bash validating data in HDFS
$ hadoop fs -ls /user/hive/warehouse                     # HDFS
$ hadoop fs -ls /user/hive/warehouse/categories/         # HDFS

$ ls -l *.avsc                                           # local, schema files
```

### Copying the schema files (`*.avsc`) to HDFS

```bash copying avro schemas to HDFS
$ sudo -u hdfs hadoop fs -mkdir /user/examples
$ sudo -u hdfs hadoop fs -chmod +rw /user/examples
$ hadoop fs -copyFromLocal ./*.avsc /user/examples/
```

NOTES: Hive and Impala

Hive and Impala both read the data from files in HDFS, and they even share metadata about the tables.

Hive - executes queries by compiling them to MapReduce jobs, this means it can be more flexible, but is much slower.

Impala - is an MPP query engine that reads the data directly from the file system itself. This allows it to execute queries fast enough for interactive analysis and exploration.

### Creating tables

tools:

- hue
- impala


Hue -> Impala: creating tables

```sql creating table in Impala
CREATE EXTERNAL TABLE categories STORED AS AVRO
LOCATION 'hdfs:///user/hive/warehouse/categories'
TBLPROPERTIES ('avro.schema.url'='hdfs://quickstart/user/examples/sqoop_import_categories.avsc');

CREATE EXTERNAL TABLE customers STORED AS AVRO
LOCATION 'hdfs:///user/hive/warehouse/customers'
TBLPROPERTIES ('avro.schema.url'='hdfs://quickstart/user/examples/sqoop_import_customers.avsc');

CREATE EXTERNAL TABLE departments STORED AS AVRO
LOCATION 'hdfs:///user/hive/warehouse/departments'
TBLPROPERTIES ('avro.schema.url'='hdfs://quickstart/user/examples/sqoop_import_departments.avsc');

CREATE EXTERNAL TABLE orders STORED AS AVRO
LOCATION 'hdfs:///user/hive/warehouse/orders'
TBLPROPERTIES ('avro.schema.url'='hdfs://quickstart/user/examples/sqoop_import_orders.avsc');

CREATE EXTERNAL TABLE order_items STORED AS AVRO
LOCATION 'hdfs:///user/hive/warehouse/order_items'
TBLPROPERTIES ('avro.schema.url'='hdfs://quickstart/user/examples/sqoop_import_order_items.avsc');

CREATE EXTERNAL TABLE products STORED AS AVRO
LOCATION 'hdfs:///user/hive/warehouse/products'
TBLPROPERTIES ('avro.schema.url'='hdfs://quickstart/user/examples/sqoop_import_products.avsc');

show tables;
```

### Exploring data

queries

```sql exploring data in Impala
-- Most popular product categories
select c.category_name, count(order_item_quantity) as count
from order_items oi
inner join products p on oi.order_item_product_id = p.product_id
inner join categories c on c.category_id = p.product_category_id
group by c.category_name
order by count desc
limit 10;


-- top 10 revenue generating products
select p.product_id, p.product_name, r.revenue
from products p 
inner join (
    select oi.order_item_product_id, sum(cast(oi.order_item_subtotal as float)) as revenue
    from order_items oi
    inner join orders o on oi.order_item_order_id = o.order_id
    where o.order_status <> ‘CANCELED’ and o.order_status <> ’SUSPECTED_FRAUD’
    group by order_item_product_id
) r on p.product_id = r.order_item_product_id
order by r.revenue desc
limit 10;
```

## Analyzing Unstructured data: log files

tools:

- flume/beeline
- hive
- impala

### Copying the log files to HDFS

```bash copy the log files to HDFS
$ sudo -u hdfs hadoop fs -mkdir /user/hive/warehouse/original_access_logs
$ sudo -u hdfs hadoop fs -copyFromLocal /opt/examples/log_files/access.log.2 /user/hive/warehouse/original_access_logs
$ hadoop fs -ls /user/hive/warehouse/original_access_logs
```

### Creating Intermediate Table

local logs -> intermediate table -> final table

```bash loading log files to Hive
$ beeline -u adbc:hive2://quickstart:10000/default -n admin -d org.apache.hive.jdbc.HiveDriver

0: jdbc:hive2://quickstart:10000/default> CREATE EXTERNAL TABLE intermediate_access_logs (
ip STRING,
date STRING,
method STRING,
url STRING,
http_version STRING,
code1 STRING,
code2 STRING,
dash STRING,
user_agent STRING)
ROW FORMAT SERDE ‘org.apache.hadoop.hive.contrib.serde2.RegexSerDe’
WITH SERDEPROPERTIES (
‘input.regex’ = ‘([^ }*) - - \\[([^\\]]*)\\] “([^\ ]*) ([^\ ]*) ([^\ ]*)” (\\d*) (\\d*) “([^”]*)” “([^”]*”’,
‘output.format.string’ = “%1$s %2$s %3$s %4$s %5$s %6$s %7$s %8$s %9$s”
)
LOCATION ‘/user/hive/warehouse/original_access_logs’;

0: jdbc:hive2://quickstart:10000/default> CREATE EXTERNAL TABLE tokenized_access_log (
ip STRING,
date STRING,
method STRING,
url STRING,
http_version STRING,
code1 STRING,
code2 STRING,
dash STRING,
user_agent STRING)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ‘,’
LOCATION ‘/user/hive/warehouse/tokenized_access_logs';

0: jdbc:hive2://quickstart:10000/default> ADD JAR /usr/lib/hive/lib/hive-contrib.jar
0: jdbc:hive2://quickstart:10000/default> INSERT OVERWRITE TABLE tokenized_access_logs SELECT * FROM intermediate_access_logs;
0: jdbc:hive2://quickstart:10000/default> !quit
```

### Validating in Impala

```sql validating tables in Impala
invalidate metadata;
show tables;

select count(*),  url from tokenized_access_logs
where url like ‘%\/product\/%’
group by url
order by count(*) desc;
```

## Analyzing data with Spark

### Start Spark

```bash start Spark
$ spark-shell —jars /usr/lib/avro/avro-mapred.jar \
—conf spark.serializer=org.apache.spark.serializer.KryoSerializer
```

### Programming in Scala

```scala relationship strengh analytics
// First we're going to import the classes we need and open some of the files
// we imported from our relational database into Hadoop with Sqoop

import org.apache.avro.generic.GenericRecord
import org.apache.avro.mapred.{AvroInputFormat, AvroWrapper}
import org.apache.hadoop.io.NullWritable

val warehouse = "hdfs://quickstart/user/hive/warehouse"

val order_items_path = warehouse + "order_items"
val order_items = sc.hadoopFile[AvroWrapper[GenericRecord], NullWritable, AvroInputFormat[GenericRecord]](order_items_path)

val products_path = warehouse + "products"
val products = sc.hadoopFile[AvroWrapper[GenericRecord], NullWritable, AvroInputFormat[GenericRecord]](product_path)



// Next, we extract the fields from order_items and products that we care about
// and get a list of every product, its name and quantity, grouped by order

val orders = order_items.map { x => (
  x._1.datum.get("order_item_product_id"),
  (x._1.datum.get("order_item_order_id"), x._1.datum.get("order_item_quantity"))
)}.join(
  products.map { x => (
    x._1.datum.get("product_id"),
    (x._1.datum.get("product_name"))
)}).map(x => (
  scala.Int.unbox(x._2._1._1), // order_id
  (
    scala.Int.unbox(x._2._1._2), // quantity
    x._2._2.toString // product_name
  )
)).groupByKey()


// Finally, we tally how many times each combination of products appears
// together in an order, and print the 10 most common combinations.

val cooccurrences = orders.map(order => (
  order._1,
  order._2.toList.combinations(2).map(order_pair => (
    if (order_pair(0)._2 < order_pair(1)._2) (order_pair(0)._2, order_pair(1)._2)
    else (order_pair(1)._2, order_pair(0)._2), order_pair(0)._1 * order_pair(1)._1
  ))
))

val combos = cooccurrrences.flatMap(x => x._2).reduceByKey((a,b) => a+b)
val mostCommon = combos.map(x => (x._2, x._1)).sortByKey(false).take(10)

println(mostCommon.deep.mkString("\n"))

exit
```

NOTE:

When we do a 'map', we specify a function that will take each record and output a modified record. This is useful when we only need a couple of fields from each record  or when we need the record to use a different field as the key: we simply invoke map with a function that takes in the entire record, and returns a new record with the fields and the key we want.

The 'reduce' operations - like 'join' and 'groupBy' - will organise these records by their keys so we can group similar records together and then process them as a group.


## Indexing data and search by Solr

### Creating a search schema

steps

- creating an empty configuration
- editing your schema
- uploading your configuration 
- creating your collection

```bash creating a search schame by Solr
$ solrctl --zk quickstart:2181/solr instancedir --generate solr_configs
$ cd /opt/examples/flume
$ solrctl --zk quickstart:2181/solr instancedir --create live_logs ./solr_configs
$ solrctl --zk quickstart:2181/solr collection --create live_logs -s 1
```

### Loading data into Solr

tools

- flume - a tool for ingesting streams of data into your cluster from sources such as log files, network streams, and more; is a system for collecting, aggregating, and moving large amounts of log data from many different sources to a centralised data source.
- morphines - a Java library for doing ETL on-the-fly.

```bash loading data by flume
$ start_logs
$ tail_logs
$ stop_logs

$ flume-ng agent \
--conf /opt/examples/flume/conf \
--conf-file /opt/examples/flume/conf/flume.conf \
--name agent1 \
-Dflume.root.logger=DEBUG,INFO,console
```

### Playing data in Solr

Hue -> Search -> Solr Search -> Dashboard

- browsing the data
- charting
