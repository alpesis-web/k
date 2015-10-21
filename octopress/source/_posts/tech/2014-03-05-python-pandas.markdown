---
layout: post_tech
title: "Python Pandas"
date: 2014-03-05 12:33:47 +0800
comments: true
categories: [tech]
tags: [python, pandas]
---

libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

### 1. object creation


```python
# create array
s = pd.Series([1,3,5,np.nan,6,8])

# create dates with dataframe
dates = pd.date_range('20130101',periods=6)

# create dataframe
df2 = pd.DataFrame({ 'header1' : 1.,
                     'header2' : pd.Timestamp('20130102'),
                     'header3' : pd.Series(1,index=list(range(4)),dtype='float32'),
                     'header4' : np.array([3] * 4,dtype='int32'),
                     'header5' : 'foo' })

# check data types of dataframe
dataframe.dtypes 
```

### 2. viewing data

```python
# top/bottom rows of data
dataframe.head()
dataframe.tail(3)

# index/row, columns and values
dataframe.index
dataframe.columns
dataframe.values

# summary of numeric data
dataframe.describe()

# data transpose
dataframe.T

# sorting by header/names and values
dataframe.sort_index(axis=1, ascending=False)
dataframe.sort(columns='columnName')

# for pandas==0.17.0
data.sort_values(by=['Name', 'Date'])
# sorted by dates and keep the first date records
first_dates = data.sort_values(by=['Date']).drop_duplicates(subset='Name', keep='first')
# sorted by dates and keep the last date records
last_dates = data.sort_values(by=['Date']).drop_duplicates(subset='Name', keep='last')
```

### 3. selection

```python
# get data by columns
dataframe['A']

# get data by rows
dataframe[0:3]
dataframe['20130102':'20130104']

# reshape dataframe
dataframe.loc[:,['A','B']]

# get one record with T
dataframe.iloc[3]

# get rows and columns as defined
dataframe.iloc[3:5,0:2]
dataframe.iloc[[1,2,4],[0,2]]
dataframe.iloc[1:3,:]
dataframe.iloc[:,1:3]

# get a specific value/cell
dataframe.iloc[1,1]
dataframe.iat[1,1]
```

### 4. boolean index

```python
# filter data by a single column
df[df.A > 0]

# filter data by all columns
df[df > 0]                   # if value <= 0, it will return NaN

# multi filters
data[(data['Date']=="2015-11-20") & (data['Change'] != "UNKNOWN")]
```


### 5. setting

```python
# set values by label, position and array
df.at[dates[0],'A'] = 0
df.iat[0,1] = 0
df.loc[:,'D'] = np.array([5] * len(df))

df2 = df.copy()
df2[df2 > 0] = -df2

# rename columns
df = df.rename(columns={'$a': 'a', '$b': 'b'}, inplace=True)
```


### 6. missing values

```python
# drop any rows those have missing values
df1.dropna(how='any')

# fill missing values
df1.fillna(value=5)

# get the boolean mask where values are nan
pd.isnull(df1)
```


### 7. operations

```python
# mean
df.mean()

# frequency count
df['A'].value_counts()

# correlation
df.corr()

# string
s.str.lower()
```


### 8. merge

```python
# merge data by rows
pieces = [df[:3], df[3:7], df[7:]]

# join
left = pd.DataFrame({'key': ['foo', 'foo'], 'lval': [1, 2]})
right = pd.DataFrame({'key': ['foo', 'foo'], 'rval': [4, 5]})
pd.merge(left, right, on='key')

# append
df = pd.DataFrame(np.random.randn(8, 4), columns=['A','B','C','D'])
s = df.iloc[3]
df.append(s, ignore_index=True)
```


### 9. grouping

```python
# (histogram)/group all columns by one column
df.groupby('A').sum()

# (cross tabs)/group all columns by multi-columns
df.groupby(['A','B']).sum()
```


### 10. reshape

```python
# stack
tuples = list(zip(*[['bar', 'bar', 'baz', 'baz', 'foo', 'foo', 'qux', 'qux'],
                    ['one', 'two', 'one', 'two', 'one', 'two', 'one', 'two']]))

index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])
df = pd.DataFrame(np.random.randn(8, 2), index=index, columns=['A', 'B'])
df2 = df[:4]
stacked = df2.stack()
stacked.unstack()
stacked.unstack(1)
stacked.unstack(0)

# pivot table
df = pd.DataFrame({'A' : ['one', 'one', 'two', 'three'] * 3,
                   'B' : ['A', 'B', 'C'] * 4,
                   'C' : ['foo', 'foo', 'foo', 'bar', 'bar', 'bar'] * 2,
                   'D' : np.random.randn(12),
                   'E' : np.random.randn(12)})
                   
pd.pivot_table(df, values='D', rows=['A', 'B'], cols=['C'])
```

### 11. get data in/out

```python
# tsv in/out
pd.read_table(dataPath)

# json in/out
pd.read_json(dataPath)

# csv in/out
pd.read_csv('foo.csv')
df.to_csv('foo.csv')

# excel in/out
pd.read_excel('foo.xlsx', 'Sheet1', index_col=None, na_values=['NA'])
df.to_excel('foo.xlsx', sheet_name='Sheet1')

# HDF5 in/out
d.read_hdf('foo.h5','df')
df.to_hdf('foo.h5','df')
```

### 12. working with columns

```python
# get features
features = [col for col in data.columns if col not in ['Id', 'Cover_Type']]

# delete columns
data = data.drop('col', 1)

# map labels
coded = {'Good': 1, 'Not Good': -1}
data['IsHoliday'] = data['IsHoliday'].astype(str).map(coded)

# split columns
def splitDatetime(data):
    sub = pd.DataFrame(data.datetime.str.split(' ').tolist(), columns = "date time".split())
    time = pd.DataFrame(sub.time.str.split(':').tolist(), columns = "hour minute second".split())
    data['date'] = sub['date']
    data['time'] = time['hour']
    return data

# shift and fillna
df['ENTRIESn_hourly'] = df['ENTRIESn'] - df['ENTRIESn'].shift()
df['ENTRIESn_hourly'] = df['ENTRIESn_hourly'].fillna(1)
```

### 13. working with rows

```python
# drop duplicates
df.drop_duplicates()
```

### 14. working with datasets

```python
# merge data
def combineStore(data, stores):
    data = pd.merge(data, stores, left_on="Store", \
                                  right_on="Store", \
                                  how="left")
    return data
```

### 15. statitics

```python
# correlation
df.corr()
```


