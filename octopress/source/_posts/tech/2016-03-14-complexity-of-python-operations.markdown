---
layout: post_tech
title: "Complexity of Python Operations"
date: 2016-03-14 21:25:46 +0800
comments: true
categories: [tech]
tags: [algorithm, python]
toc: true
---

Types of the operations:

- lists
- sets
- dictionary

## Lists

| Operation     | Example      | Class         | Notes                             |
|---------------|--------------|---------------|-----------------------------------|
| Index         | l[i]         | O(1)	       |                                   |
| Store         | l[i] = 0     | O(1)	       |                                   |
| Length        | len(l)       | O(1)	       |                                   |
| Append        | l.append(5)  | O(1)	       |                                   |
| Pop	        | l.pop()      | O(1)	       | same as l.pop(-1), popping at end |
| Clear         | l.clear()    | O(1)	       | similar to l = []                 |
| Slice         | l[a:b]       | O(b-a)	       | l[1:5]:O(l)/l[:]:O(len(l)-0)=O(N) |
| Extend        | l.extend(...)| O(len(...))   | depends only on len of extension  |
| Construction  | list(...)    | O(len(...))   | depends on length of argument     |
| check ==, !=  | l1 == l2     | O(N)          |                                   |
| Insert        | l[a:b] = ... | O(N)	       |                                   |
| Delete        | del l[i]     | O(N)	       |                                   |
| Remove        | l.remove(...)| O(N)	       |                                   |
| Containment   | x in/not in l| O(N)	       | searches list                     |
| Copy          | l.copy()     | O(N)	       | Same as l[:] which is O(N)        |
| Pop	        | l.pop(0)     | O(N)	       |                                   |
| Extreme value | min(l)/max(l)| O(N)	       |                                   |
| Reverse       | l.reverse()  | O(N)	       |                                   |
| Iteration     | for v in l:  | O(N)          |                                   |
| Sort          | l.sort()     | O(N Log N)    | key/reverse doesn't change this   |
| Multiply      | k*l          | O(k N)        | 5*l is O(N): len(l)*l is O(N**2)  |


## Sets

| Operation     | Example      | Class         | Notes                         |
|---------------|--------------|---------------|-------------------------------|
| Length        | len(s)       | O(1)	       |                               |
| Add           | s.add(5)     | O(1)	       |                               |
| Containment   | x in/not in s| O(1)	       | compare to list/tuple - O(N)  |
| Remove        | s.remove(5)  | O(1)	       | compare to list/tuple - O(N)  |
| Discard       | s.discard(5) | O(1)	       |                               |
| Pop           | s.pop()      | O(1)	       | compare to list - O(N)        |
| Clear         | s.clear()    | O(1)	       | similar to s = set()          |
| Construction  | set(...)     | len(...)      |                               |
| check ==, !=  | s != t       | O(min(len(s),lent(t))|                        |
| <=/<          | s <= t       | O(len(s1))    | issubset                      |
| >=/>          | s >= t       | O(len(s2))    | issuperset s <= t == t >= s   |
| Union         | s | t        | O(len(s)+len(t))|                             |
| Intersection  | s & t        | O(min(len(s),lent(t))|                        |
| Difference    | s - t        | O(len(t))     |                               |
| Symmetric Diff| s ^ t        | O(len(s))     |                               |
| Iteration     | for v in s:  | O(N)          |                               |
| Copy          | s.copy()     | O(N)	       |                               |


## Dictionary

| Operation     | Example      | Class         | Notes                         |
|---------------|--------------|---------------|-------------------------------|
| Index         | d[k]         | O(1)	       |                               |
| Store         | d[k] = v     | O(1)	       |                               |
| Length        | len(d)       | O(1)	       |                               |
| Delete        | del d[k]     | O(1)	       |                               |
| get/setdefault| d.method     | O(1)	       |                               |
| Pop           | d.pop(k)     | O(1)	       |                               |
| Pop item      | d.popitem()  | O(1)	       |                               |
| Clear         | d.clear()    | O(1)	       | similar to s = {} or = dict() |
| Views         | d.keys()     | O(1)	       |                               |
| Construction  | dict(...)    | len(...)      |                               |
| Iteration     | for k in d:  | O(N)          | all forms: keys, values, items|


## References

- [Complexity of Python Operations](https://www.ics.uci.edu/~pattis/ICS-33/lectures/complexitypython.txt)
