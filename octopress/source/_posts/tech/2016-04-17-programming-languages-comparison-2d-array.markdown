---
layout: post_tech
title: "Programming Languages Comparison: 2D Array"
date: 2016-04-17 03:32:32 +0800
comments: true
categories: [tech]
tags: [c, c++, java, python, programming]
toc: true
---

## Task

- Given a 2D array
- Calulate the hourglass: val = a[i-1][j-1] + a[i-1][j] + a[i-1][j+1] + a[i][j] + a[i+1][j-1] + a[i+1][j] + a[i+1][j+1]
- Print the maxVal

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
    int arr[6][6];
    for(int arr_i = 0; arr_i < 6; arr_i++){
       for(int arr_j = 0; arr_j < 6; arr_j++){
          
          scanf("%d",&arr[arr_i][arr_j]);
       }
    }
    
    int val = 0;
    int maxVal = -63;
    int arrLen = sizeof(arr)/sizeof(arr[0]);
    
    for (int i=1; i<=arrLen-2; i++){
        for (int j=1; j<=arrLen-2; j++){
            val = arr[i-1][j-1] + arr[i-1][j] + arr[i-1][j+1] + arr[i][j] + arr[i+1][j-1] + arr[i+1][j] + arr[i+1][j+1];
            if (val > maxVal){
                maxVal = val;
            }
        }
    }
    
    printf("%d\n", maxVal);
    
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
    vector< vector<int> > arr(6,vector<int>(6));
    for(int arr_i = 0;arr_i < 6;arr_i++){
       for(int arr_j = 0;arr_j < 6;arr_j++){
          cin >> arr[arr_i][arr_j];
       }
    }
    
    int val = 0;
    int maxVal = -63;
    int arrLen = arr.size();
    for (int i=1; i<=arrLen-2; i++) {
        for (int j=1; j<=arrLen-2; j++){
            val = arr[i-1][j-1] + arr[i-1][j] + arr[i-1][j+1] + arr[i][j] + arr[i+1][j-1] + arr[i+1][j] + arr[i+1][j+1];
            if (val > maxVal) {
                maxVal = val;
            }
        }
    }

    cout << maxVal << endl;
    
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
        int arr[][] = new int[6][6];
        for(int i=0; i < 6; i++){
            for(int j=0; j < 6; j++){
                arr[i][j] = in.nextInt();
            }
        }
        
        int curSum = 0;
        int maxSum = -63;
        int arrLen = arr.length;
        
        for (int i=1; i<=(arrLen-2); i++) {
            for (int j=1; j<=(arrLen-2); j++) {
                curSum = arr[i-1][j-1] + arr[i-1][j] + arr[i-1][j+1] + arr[i][j] + arr[i+1][j-1] + arr[i+1][j] + arr[i+1][j+1];
                if (curSum > maxSum) {
                    maxSum = curSum;
                }
            }
        }
        
        System.out.println(maxSum);
    }
}
```

### Python

```python
#!/bin/python

import sys


arr = []
for arr_i in xrange(6):
   arr_temp = map(int,raw_input().strip().split(' '))
   arr.append(arr_temp)
    
    
maxVal = -63    
for i in range(1, len(arr)-1, 1):
    for j in range(1, len(arr)-1, 1):
        val = arr[i-1][j-1] + arr[i-1][j] + arr[i-1][j+1] + arr[i][j] + arr[i+1][j-1] + arr[i+1][j] + arr[i+1][j+1]
        if val > maxVal:
            maxVal = val
print maxVal
```
