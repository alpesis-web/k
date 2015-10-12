---
layout: post_tech
title: "Python Syntax"
date: 2014-05-23 13:21:29 +0800
comments: true
categories: [tech]
tags: [python]
---

## 1. Default Functions

### 1.1. string

```python
# check subset
if "xxx" in aString:
    has_string = True

# join two sets
set1 = ['a', 'b', 'c']
set2 = ['b', 'c', 'd']
fullset = set(list(set1) + list(set2))
fullset = list(set(set1 + set2))

# remove empty strings
a = 'abc  '
print len(a)
print len(a.strip())
```

### 1.2. IO

```python
# output with csv
with open(outPath+"submission-randomforest.csv", "wb") as outfile:
    outfile.write("Id,Cover_Type\n")
    for index, value in enumerate(list(clf.predict(test[cols]))):
        outfile.write("%s,%s\n" % (test['Id'].loc[index], value))
```

### 1.3. random

```python
random.uniform(10, 20)  #   a <= n <= b  

random.random()  #  0 <= n < 1.0
random.randint(12, 20)  
random.randrange(10, 100, 2)  # [10, 12, 14, 16, ... 96, 98]
random.choice(range(10, 100, 2))
random.choice(["JGood", "is", "a", "handsome", "boy"])  

p = ["Python", "is", "powerful", "simple", "and so on..."]  
random.shuffle(p) 

list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]  
slice = random.sample(list, 5)
```

## 2. Default Packages

### 2.1. datetime

```python
import datetime

# calculating days: minus
d1 = datetime.datetime(2009, 3, 23)
d2 = datetime.datetime(2009, 10, 7)
(d1 - d2).days

# calculating days: plus
d1 = datetime.datetime.now()
d3 = d1 + datetime.timedelta(days=10)
d3.ctime()

# calculating seconds
starttime = datetime.datetime.now()
endtime = datetime.datetime.now()
(endtime - starttime).seconds
```

### 2.2. JSON

json

```python
import json
from pprint import pprint

jsonfile = open(dataPath + fileName)
data = json.load(jsonfile)
pprint(data)
jsonfile.close()
```

simplejson

```python
import simplejson as json

print(json.dumps(jsonData, sort_keys=True, indent=4 * ' '))
```

### 2.3. os

```python
import os

# get the current url of the file folder
outputs = os.path.join(os.path.dirname(os.path.abspath(__file__)), 'outputs')
```
