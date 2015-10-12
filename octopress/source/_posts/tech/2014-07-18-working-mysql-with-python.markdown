---
layout: post_tech
title: "Working MySQL with Python"
date: 2014-07-18 13:12:22 +0800
comments: true
categories: [tech]
tags: [mysql, python]
---

## 1. Connection

```python
""" python with MySQL """

import MySQLdb

# connect database
conn = MySQLdb.connect(host='localhost', user='root', passwd='pw_root', db='test')
cursor = conn.cursor()  # opening cursor

n = cursor.execute('select * from table')  # querying with SQL, return # of rows
data = cursor.fetchall()  # getting all records

# connection closed
cursor.close()
conn.close()
```

## 2. Processing

```python
import pandas
import MySQLdb

def connectMySQL(host, user, passwd, db, sql):
    """ connect MySQL and return data by sql query"""

    try:
        conn = MySQLdb.connect(host, user, passwd, db)
        cursor = conn.cursor()

        n = cursor.execute(sql)  # querying with sql, return # of rows
        data = cursor.fetchall()  # fetching all rows

        cursor.close()
        conn.close()

        return data

    except Exception, e:
        print "MySQL server could not be connected."

        
def createDataFrame(data):
    """ convert data into pandas data frame """

    days = []
    visits = []
    orders = []
    for row in data:
        day, visit, order = row  # splitting columns
        days.append(day)
        visits.append(visit)
        orders.append(order)
        
    daily_orders = pandas.DataFrame({'Days': days,
                                     'Visits': visits,
                                     'Orders': orders
                                     })    
    return daily_orders


def main():
    sql = "SELECT * FROM reporting.daily_orders WHERE reporting.daily_orders.Days = '2013-01-12'"
    data = connectMySQL('localhost', 'root', 'pw_root', 'test', sql)
    daily_orders = createDataFrame(data)


if __name__ == '__main__':
    main()
```

## 3. Source Codes

```python
import MySQLdb

class Database:
    """ creating the class Database """

    def __init__(self, host, user, passwd, db):
        """ initializing database and connecting """
        
        self.host = host
        self.user = user
        self.passwd = passwd
        self.db = db

        self.conn = MySQLdb.connect(host=self.host, user=self.user, passwd=self.passwd, db=self.db)
        self.cursor = self.conn.cursor()

    def insert(self, sql):
        """ inserting records into database """
        
        try:
            self.cursor.execute(sql)
            self.conn.commit()
        except:
            self.conn.rollback()

    def query(self, sql):
        """ querying data with sql """
        
        cursor = self.conn.cursor()
        cursor.execute(sql)

        return cursor.fetchall()

    def close(self):
        """ closing cursor and connection """
        
        self.cursor.close()
        self.conn.close()


def mergeData(dataList):
    """ merging all country data into one """
    
    fullData = []
    for data in dataList:
        fullData.extend(data)
    return fullData


def main():
    
    # creating the instances of database
    dbMY = Database('localhost', 'root', 'pw_root', 'zlmy_reporting')
    dbSG = Database('localhost', 'root', 'pw_root', 'zlsg_reporting')
    dbRE = Database('localhost', 'root', 'pw_root', 'zlre_reporting')

    # querying data from database
    dataMY = db1.query('SELECT * from orders')
    dataSG = db2.query('SELECT * from orders')

    # merging data into one
    data = mergeData([dataMY, dataSG])

    # creating new table if not exists in the database zlre_reporting
    sql = """
             CREATE TABLE IF NOT EXISTS orders (
               Days  VARCHAR(10),
               Order_nr  INT(20),
               Price_paid  FLOAT
             )
         """
    dbRE.query(sql)

    # inserting records into the database zlre_reporting
    sql = """
             INSERT INTO orders (Days, Order_nr, Price_paid) VALUES ('%s', '%s', '%s')
          """
    for row in data:
        db3.insert(sql % (row[0], row[1], row[2]))
    
    
    # closing database
    dbMY.close()
    dbSG.close()
    dbRE.close()
 

if __name__ == '__main__':
    main()
```
