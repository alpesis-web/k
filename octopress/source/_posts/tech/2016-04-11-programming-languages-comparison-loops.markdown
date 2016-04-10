---
layout: post_tech
title: "Programming Languages Comparison: Loops"
date: 2016-04-11 00:43:16 +0800
comments: true
categories: [tech]
tags: [c, c++, java, python, programming]
toc: true
---

## Task

- Calculate N * [1..10]
- Print the result

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
    
    int endOfRange = 10;
    for (int i=1; i <= endOfRange; i++) {
        int product = N * i;
        printf("%d x %d = %d\n", N, i, product);
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
#include <unordered_map>

using namespace std;


int main(){
    int N;
    cin >> N;
    
    int endOfRange = 10;
    for (int i=1; i <= endOfRange; i++) {
        int product = N * i;
        printf("%d x %d = %d\n", N, i, product);
    }
    
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
        Scanner in = new Scanner(System.in);
        int N = in.nextInt();
        
        int endOfRange = 10;
        int product;
        for (int i = 1; i <= endOfRange; i++) { 
            product = N * i;
            System.out.printf("%d x %d = %d\n", N, i, product);
        }
        
    }
}
```

### Python

Python 2

```python
#!/bin/python

import sys


N = int(raw_input().strip())

for num in range(10):
    print "%d x %d = %d" % (N, num+1, N*(num+1))
```
