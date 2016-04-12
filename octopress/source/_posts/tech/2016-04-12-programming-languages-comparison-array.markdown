---
layout: post_tech
title: "Programming Languages Comparison: Array"
date: 2016-04-12 23:29:46 +0800
comments: true
categories: [tech]
tags: [c, c++, java, python, programming]
toc: true
---

## Task

- Reverse an array.

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
    int n; 
    scanf("%d",&n);
    int *arr = malloc(sizeof(int) * n);
    for(int arr_i = 0; arr_i < n; arr_i++){
       scanf("%d",&arr[arr_i]);
    }
    
    for (int arr_i=n-1; arr_i>=0; arr_i--){
        printf("%d ", arr[arr_i]);
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
    int n;
    cin >> n;
    vector<int> arr(n);
    for(int arr_i = 0;arr_i < n;arr_i++){
       cin >> arr[arr_i];
    }
    
    for (int arr_i = n-1; arr_i >=0; arr_i--){
        printf("%d ", arr[arr_i]);
    }
    return 0;
}
```

### Java

```java
mport java.io.*;
import java.util.*;


public class Solution {

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int[] arr = new int[n];
        for(int i=0; i < n; i++){
            arr[i] = in.nextInt();
        }
        for (int i=n-1; i >= 0; i--) {
            System.out.print(arr[i]+" ");
        }
        
        in.close();
    }
}
```

### Python

```python
#!/bin/python

import sys


n = int(raw_input().strip())
arr = map(int,raw_input().strip().split(' '))

print " ".join(map(str, arr[::-1]))
```
