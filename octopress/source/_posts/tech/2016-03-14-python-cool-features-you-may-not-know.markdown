---
layout: post_tech
title: "Python Cool Features You May Not Know"
date: 2016-03-14 22:22:48 +0800
comments: true
categories: [tech]
tags: [python]
toc: true
---

## Lists

### Reverse

```python
>>> num_list = [1,2,3]
>>> num_list.reverse()
>>> num_list
[3, 2, 1]
```

### Set

```python
>>> num_list = [1,2,3,3,2,1]
>>> set(num_list)
set([1, 2, 3])
```

### iter

```python
>>> l = [1,2,3]
>>> iter(l)
<listiterator object at 0x106caab90>
>>> [i for i in iter(l)]
[1, 2, 3]
```

### Zip

This function returns a list of tuples, where the i-th tuple contains the i-th element from each of the argument sequences or iterables.

```python
>>> x = [1,2,4,5]
>>> y = [1,2,3]
>>> zip(x,y)
[(1, 1), (2, 2), (4, 3)]

>>> num_list = [1,2,3,4,5,6]
>>> zip(*[iter(num_list)] * 2)
[(1, 2), (3, 4), (5, 6)]
>>> zip(*[iter(num_list)] * 3)
[(1, 2, 3), (4, 5, 6)]
>>> zip(*[iter(num_list)] * 4)
[(1, 2, 3, 4)]

>>> def make_ngrams(alist, n):
...     return zip(*(alist[i:] for i in xrange(n)))
... 
>>> make_ngrams([1,2,3,4,5,6],2)
[(1, 2), (2, 3), (3, 4), (4, 5), (5, 6)]
>>> make_ngrams([1,2,3,4,5,6],3)
[(1, 2, 3), (2, 3, 4), (3, 4, 5), (4, 5, 6)]
```

### Join

```python
>>> names = ['John', 'Bob', 'Alice']
>>> '+'.join(names)
'John+Bob+Alice'
```

### Next

```python
>>> num_list = (x ** 2 for x in xrange(10))
>>> next(num_list)
0
>>> next(num_list)
1
>>> next(num_list)
4
>>> next(num_list)
9
>>> next(num_list)
16
>>> next(num_list)
25
```

### Unpacking

```python
>>> def foo(a,b,c):
...     print a,b,c
... 
>>> mydict = {'a': 1, 'b': 2, 'c': 3}
mylist = ['John', 'Bob', 'Alice']

>>> foo(*mydict)
a c b
>>> foo(**mydict)
1 2 3
>>> foo(*mylist)
John Bob Alice
```

## Libraries

### import this

```python
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```
