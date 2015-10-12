---
layout: post_tech
title: "Printing A Schedule"
date: 2014-07-18 13:19:22 +0800
comments: true
categories: [tech]
tags: [python]
---

### The tennis problem

A tennis club is organizing a fun tournament once a year. We have 2n players, which are divided in two groups. We have the good players A = {a1, …, an} and the bad players B = {b1, …, bn}.

The tournament is a double tournament and the teams consist always of one good player (group A) and one bad player (group B). i. E.g., one match could be (a_1, b_2) vs. (a_3, b_4)

The playing schedule has to follow some rules:

- All players of group A have to play against each other (teams play in a group, no knockout tournament).
- The same for the players of group B.
- The team (ai, bj) only plays together once during the tournament (this means, you never play twice with the same partner).
- The players of the same rank can’t play together (the best player of A never plays together with the best player of B etc.) i.e., we never have the team (ai, bi).

“Pretty print” the schedule of the tournament for any given n.


### Solution

```python
""" solution without constraint """

def createSchedule(n):

    A = ['a' + str(x+1) for x in range(n)]  # creating good players
    B = ['b' + str(x+1) for x in range(n)]  # creating bad players

    # generating schedule
    for i in range(n-1):
        for j in range(i+1, n):
            print "(%s, %s) vs. (%s, %s)" % (A[i], B[j], A[j], B[i])

createSchedule(5)
```


### Output

```
(a1, b2) vs. (a2, b1)
(a1, b3) vs. (a3, b1)
(a1, b4) vs. (a4, b1)
(a1, b5) vs. (a5, b1)
(a2, b3) vs. (a3, b2)
(a2, b4) vs. (a4, b2)
(a2, b5) vs. (a5, b2)
(a3, b4) vs. (a4, b3)
(a3, b5) vs. (a5, b3)
(a4, b5) vs. (a5, b4)
```
