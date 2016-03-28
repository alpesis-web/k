---
layout: post_tech
title: "An Array To Balanced Binary Tree"
date: 2016-03-15 02:02:23 +0800
comments: true
categories: [tech]
tags: [algorithm, python]
toc: false
---

Write a function to convert a non-decreasing list of ints to a balanced binary tree.

```python
Input:  Linked List 1->2->3
Output: A Balanced BST
     2
   /  \
  1    3


Input: Linked List 1->2->3->4->5->6->7
Output: A Balanced BST
        4
      /   \
     2     6
   /  \   / \
  1   3  5   7

Preorder traversal of constructed BST
4 2 1 3 6 5 7

Input: Linked List 1->2->3->4
Output: A Balanced BST
      3
    /  \
   2    4
 /
1

Input:  Linked List 1->2->3->4->5->6
Output: A Balanced BST
      4
    /   \
   2     6
 /  \   /
1   3  5


1) Get the Middle of the linked list and make it root.
2) Recursively do same for left half and right half.
       a) Get the middle of left half and make it left child of the root
          created in step 1.
       b) Get the middle of right half and make it right child of the
          root created in step 1.

```

Coding

```python
class Node:

    def __init__(self, data):
        self.left = None
        self.right = None
        self.data = data



def list2BST(num_list, start, end):
    if start > end:
        return None

    middle = (start + end) / 2
    #print start, end, middle
    node = Node(num_list[middle])

    node.left = list2BST(num_list, start, middle-1)
    node.right = list2BST(num_list, middle+1, end)
    return node

def traverse(node):
    if node == None:
        return;
    print str(node.data)
    traverse(node.left)
    traverse(node.right)


if __name__ == '__main__':

    num_list = [1,2,3,4,5,6,7]
    list_len = len(num_list)

    root = list2BST(num_list, 0, list_len-1)
    traverse(root)
```

Time Complexity: O(n)

```
  T(n) = 2T(n/2) + C
  T(n) -->  Time taken for an array of size n
   C   -->  Constant (Finding middle of array and linking root to left 
                      and right subtrees take constant time) 
```
