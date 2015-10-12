---
layout: post_tech
title:  Binary Search 
date:   2014-07-29 22:33:00 +0800
comments: true
categories: [tech]
tags: [python, algorithm]
---

## Introduction

A binary search, it also called half-interval search which finds the position
of a speicified input value (key) within an array sorted by key value. The
order of the array should be arranged in ascending or descending.

## Algorithm

NOTE: the array must be a ordered list

In each step, the algorithm compares the search key value with the key value of
the middle element in the array.

- If the array is empty, return "not found".
- Else,
    - If the keys match, then a matching element has been found and its
  index/position is returned.
    - Else, 
        - if the search key is less than the middle element's key, then the
      algorithm repeats its action on the sub-array to the left of the middle
      lement.
        - if the search key is greater, on the sub-array to the right.


## Pros and Cons

Pros:

- the times of comparison is less
- the speech of search is fast
- the average performance is good 


Cons:

- the array must be a ordered list
- insertion / deletion is difficult

Applications:

- the value in the array could not be modified frequently
- the value in the array is searched frequently


## Source Codes

```python
def binary_search(ordered_list, item):

    first_pos = 0 
    last_pos = len(ordered_list) - 1
    found = False

    if first_pos < last_pos and not found:
        midpoint = (first_pos + last_pos) // 2
        #print midpoint

        if ordered_list[midpoint] == item:
            found = True
        else:
            if item < ordered_list[midpoint]:
                last_pos = midpoint - 1
            else:
                first_pos = midpoint + 1
    
    return found
```

It also can be recursive.

```python
def binary_search(ordered_list, item):

    if len(ordered_list) == 0:
        return False
    else:
        midpoint = len(ordered_list) // 2
        if ordered_list[midpoint] == item:
            return True
        else:
            if item < ordered_list[midpoint]:
                return binary_search(ordered_list[:midpoint], item)
            else:
                return binary_search(ordered_list[midpoint+1:], item)
```

