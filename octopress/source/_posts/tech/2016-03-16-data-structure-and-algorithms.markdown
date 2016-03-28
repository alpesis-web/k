---
layout: post_tech
title: "Data Structure and Algorithms"
date: 2016-03-16 19:28:11 +0800
comments: true
categories: [tech]
tags: [algorithm, python]
toc: true
---

Compositions: Graph <- Tree <- List

- Graph
- Tree: Binary Tree, Heap, Hash
- List: Array, Stack, Queue

Case Study:

Supose we want to design a system for storing employee records keyed using phone numbers.
And we want following queries to be performed efficiently:

- insert a phone number and corresponding information
- search a phone number and fetch the information
- delete a phone number and related information

We can think of using the following data structures to maintain information about different
phone numbers.

- array of phone numbers and records
- linked list of phone numbers and records
- balanced binary search tree with phone number as keys
- direct access table

Comparisons:

- **arrays and linked lists**: if search in a linear fashion, which can be costly in practice. If we
use arrays and keep the data sorted, then a phone number can be searched in O(logn) time using
Binary Search, but insert and delete operations become costly as we have to maintain sorted
order. 
- **balanced binary search tree**: we get moderate search, insert and elete times. All
of these operations can be guaranteed to be in O(logn) time.
- **direct access table**: we make a big array and use phone numbers as index in the array. An entry
in array is NIL if phone number is not present, else the array entry stores pointer to records
corresponding to phone number. Time complexity wise this solution is the best among all, we
can do all operations in O(1) time. For example, to insert a phone number, we create a record
with details of given phone number, use phone number as index and store the pointer to the
created record in table. This solution has many practical limitations. First problem with this
solution is extra space required is huge. For example, if phone number is n digits, we need
O(m * 10n) spae for table where m is size of a pointer to record. Another problem is an integer
in a programming language may not store n digits.
- **Hashing**: we get O(1) search time on average (unser reasonable assumptions) and O(n) in worst
case. Hashing is an improvement over Direct Access Table. The idea is to use hash function that
converts a given phone number or any other key to a smaller number and uses the small number as
index in a table called hash table.


## Graph

### Graph

#### Data Structure 

```python
"""
Graph

	{'A': {'B'},
	 'B': {'D', 'C'},
	 'C': {'D'},
	 'E': {'F'},
	 'F': {'C'}}
"""

from collections import defaultdict

class Graph(object):
    """
    Graph data structure, undirected by default.
    """

    def __init__(self, connections, directed=False):
        self._graph = defaultdict(set)
        self._directed = directed
        self.add_connections(connections)
```


#### Algorithms

- DFS/BFS
- Minimum Spanning Tree
- Shortest Paths
- Connectivity
- Hard Problems
- Maximum Flow
- Misc

### Hash

#### Data Structure

Hash Function:

A function that converts a given big phone number to a small practical integer value.
The mappd integer value is used as an index in hash table. In simple terms, a hash function
maps a big number or string to a small integer that can be used as index in hash table.

A good hash function should have following properties:

- efficiently computable
- should uniformly distribute the keys (each table position equally likely for each key)

For example, for phone numbers, a bad hash function is to take first three digits, a better
function is consider last three digits, please note that this may not be the best hash function.
There may be better ways.

Hash Table:

An array that stores pointers to records corresponding to a given phone number. An entry
in hash table is NIL if no existing phone number has hash function value equal to the
index for the entry.

Collision Handling

Since a hash function gets us a small number for a key which is a big integer or string,
there is possibility that two keys result in same value. The situation where a newly inserted
key maps to an already occupied slot in hash table is called collision and must be handled using
some collision handling technique. Following are the ways to handle collisioins:

- chaining: the idea is to make each cell of hash table point to a linked list of records that
have same hash function value. Chaining is simple, but requires additional memory outside
the table.
- Open Addressing: in open addressing, all elements are stored in the hash table itself. Each
table entry contains either a record or NIL. When searching for an element, we one by one examine
table slots until the desired element is found or it is clear that the element is not in the table. 



#### Algorithms

## Tree

### Binary Tree

#### Data Structure

Properties

- the left subtree of a node contains only nodes with keys less than the node's key
- the right subtree of a node contains only nodes with keys greater than the node's key
- the left and right subtree each must also be a binary search tree
- there must be no duplicate nodes

```python
"""
Binary Tree:

           1
       /       \
      2          3
    /   \       /  \
   4    None  None  None
  /  \
None None

"""

class Node:

    def __init__(self, key):
        self.left = None
        self.right = None
        self.val = key

if __name__ == '__main__':

    root = Node(1)
    root.left = Node(2)
    root.right = Node(3)
    root.left.left = Node(4)
```

#### Algorithms

Greedy algorithms

- Activity selectioin problem
- Kruskal's minimum spanning tree 
- Prim's minimum spanning tree
- Prim's MST for Adjacency list representation
- Dijkstra's shortest path algorithm
- Dijkstra's algorithm for Adjacency list representation
- Huffman coding
- Efficient Huffman coding for sorted input


### Binary Heap

#### Data Structure

Properties:

- It is a complete tree (All levels are completely filled except possibly the last level and
the last level has all keys as left as possible)> This property of Binary Heap makes them
suitable to be stored in an array.
- A binary heap is either Min Heap or Max Heap. In a Min Binary Heap, the key at root must be
minimum among all keys present in Binary Heap. The same property must be recursively true for
all nodes in Binary Tree. Max Binary Heap is similar to Min Heap.

```
            10                      10
         /      \               /       \  
       20        100          15         30  
      /                      /  \        /  \
    30                     40    50    100   40
```

#### Algorithms



## Lists

### LinkedList

#### Data Structure

```python
"""
Linked List

  DS

       LinkedList       First Node          Nodes

       +------+         +------+------+     +------+------+
       | head | <-----  | data | next | <-- | data | next |
       +------+         +------+------+     +------+------+

  e.g.

    class Node

        +----+------+    +----+------+    +----+------+
        | 1  | None |    | 2  | None |    | 3  | None |
        +----+------+    +----+------+    +----+------+

    class LinkedList

        +----+------+    +----+------+    +----+------+
        | 1  |    -----> | 2  |   ------> | 3  | None |
        +----+------+    +----+------+    +----+------+
"""


class LinkedList:

    def __init__(self):
        self.head = None

    def traverse(self):
        temp = self.head
        while (temp):
            print temp.data,
            temp = temp.next


class Node:

    def __init__(self, data):
        self.data = data
        self.next = None


if __name__ == '__main__':

    first = Node(1)
    second = Node(2)
    third = Node(3)

    linked_list = LinkedList()
    linked_list.head = first
    linked_list.head.next = second
    second.next = third

    linked_list.traverse()
```

#### Algorithm

### Stack

#### Data Structure

```python
"""
Stack
"""

from sys import maxsize

def createStack():
    stack = []
    return stack

def isEmpty(stack):
    return len(stack) == 0

def push(stack, item):
    stack.append(item)
    print("pushed to stack " + item)

def pop(stack):
    if (isEmpty(stack)):
        return str(-maxsize -1)  # return minus infinite

    return stack.pop()

def peek(stack):
    if (isEmpty(stack)):
        return str(-maxsize -1)

    return stack[len(stack) - 1]


if __name__ == '__main__':

    stack = createStack()

    push(stack, str(10))
    push(stack, str(20))
    push(stack, str(30))

    print(pop(stack) + " popped from stack")
    print("Top item is " + peek(stack))
```

#### Algorithms

### Queue

#### Data Structure

#### Algorithms

### Array

#### Data Structure

```python
people = ['Bob', 'Alice', 'John']
```

#### Algorithms

searching

- binary search

sorting

- selection sort
- bubble sort
- insertion sort
- merge sort
- heap sort
- quick sort
- radix sort
- counting sort
- bucket sort
- shell sort
- comb sort
- pigeonhole sort


## References

- [Geeks for Geeks](http://www.geeksforgeeks.org)
