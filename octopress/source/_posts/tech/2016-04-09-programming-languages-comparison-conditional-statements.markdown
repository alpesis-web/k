---
layout: post_tech
title: "Programming Languages Comparison: Conditional Statements"
date: 2016-04-09 00:05:56 +0800
comments: true
categories: [tech]
tags: [programming, c, c++, python, java]
toc: true
---

## Task

Given an integer n, perform the following conditonal actions:

- if n is odd, print `Weird`;
- if n is even, and in the inclusive range of 2 to 5, print `Not Weird`;
- if n is even, and in the inclusive range of 6 to 20, print `Weird`;
- if n is even, and greater than 20, print `Not Weird`.

## Solutions

### C

```c
#include <math.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <assert.h>
#include <limits.h>
#include <stdbool.h>

int main(){
    int N; 
    scanf("%d",&N);
    
    if (N%2==1){
        printf("Weird");
    }
    else{
        if (N%2==0 && N>=6 && N<=20){
            printf("Weird");
        }
        else{
            printf("Not Weird");
        }
    }
    
    return 0;
}
```

### C++

```cpp
#include <map>
#include <set>
#include <list>
#include <cmath>
#include <ctime>
#include <deque>
#include <queue>
#include <stack>
#include <string>
#include <bitset>
#include <cstdio>
#include <limits>
#include <vector>
#include <climits>
#include <cstring>
#include <cstdlib>
#include <fstream>
#include <numeric>
#include <sstream>
#include <iostream>
#include <algorithm>
using namespace std;


int main(){
    int N;
    cin >> N;
    
    string ans = "";
    if (N%2==1){
        ans = "Weird";
    }
    else {
        if (N%2==0 && (N >= 6 && N <= 20)){
            ans = "Weird";
        }
        else{
            ans = "Not Weird";
        }
    }
    cout << ans << endl;
    
    return 0;
}
```

### Java

```java
import java.io.*;
import java.util.*;
import java.text.*;
import java.math.*;
import java.util.regex.*;
public class Solution {
   
   public static void main(String[] args) {
      Scanner scan = new Scanner(System.in);
      int n = scan.nextInt(); 
      scan.close();
      String ans="";
          
      // if 'n' is NOT evenly divisible by 2 (i.e.: n is odd)
      if(n%2==1){
         ans = "Weird";
      }
      else{
         // Complete the code 
         if (n%2==0 && (n >=6 && n <=20)){
             ans = "Weird";
         }
         else {
             ans = "Not Weird";
         }
      }
      System.out.println(ans);
   }
}
```

### Python

Python 2

```python
#!/bin/python

import sys


N = int(raw_input().strip())

if N % 2 == 0 and (2 <= N <= 5 or N > 20):
    print "Not Weird"
else:
    print "Weird"
```

Python 3

```python
#!/bin/python3

import sys


N = int(input().strip())

if N % 2 == 0 and (2 <= N <= 5 or N > 20):
    print("Not Weird")
else:
    print("Weird")
```
