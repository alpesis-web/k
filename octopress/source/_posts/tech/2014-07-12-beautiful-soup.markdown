---
layout: post_tech
title: "Beautiful Soup"
date: 2014-07-12 17:56:44 +0800
comments: true
categories: [tech]
tags: [python, beautifulsoup]
---

## 1. Connection

```python
import urllib2
from bs4 import BeautifulSoup

url = "..."
html = urllib2.urlopen(url).read()
soup = BeautifulSoup(html)
```

## 2. Extraction

### 2.1. HTML

extraction from tags

```python
soup.head
soup.head.contents
soup.head.contents[0]

soup.title
soup.body.b
soup.a
```

get url

```python
for link in soup.find_all('a'):
    print(link.get('href'))
```

find contents

```python
soup.find_all("title")
soup.find_all("p", "title")
soup.find_all(id="link2")
soup.findAll('a', attrs={'class': 'results'})


import re
soup.find(text=re.compile("sisters"))
```

### 2.2. Regular Expressions

extraction with re

```python
import re
for tag in soup.find_all(re.compile("^b")):
    print(tag.name)
```

## Reference

- [BeautifulSoup Documentation](http://www.crummy.com/software/BeautifulSoup/bs4/doc/)
