---
layout: post_tech
title: "Programming Languages Comparison: Recursion"
date: 2016-04-14 23:15:50 +0800
comments: true
categories: [tech]
tags: [c, c++, java, python, programming]
toc: true
---

## Task

- implement the factorial N = 1 if N <= 1, else N * factorial(n-1)

## Solutions

### C

```c
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

int factorial(int n) {
    if (n == 1) {
        return 1;
    }
    else {
        return n * factorial(n-1);
    }
}

int main() {

    /* Enter your code here. Read input from STDIN. Print output to STDOUT */ 
    int n;
    scanf("%d", &n);
    printf("%d\n", factorial(n));
    
    return 0;
}
```

### C++

```cpp
#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;

int factorial(int n) {
    if (n == 1) {
        return 1;
    }
    else {
        return n * factorial(n-1);
    }
}

int main() {
    /* Enter your code here. Read input from STDIN. Print output to STDOUT */
    int n;
    cin >> n;
    cout << factorial(n) << endl;
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
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution.
        */
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        System.out.println(factorial(n));
    }
    
    private static int factorial(int n) {
        if (n == 1){
            return 1;
        }
        else {
            return n * factorial(n-1);
        }
    }
}
```

### Python

```python
def factorial(n):
    
    if n == 1:
        return 1
    else:
        return n * factorial(n-1)

n = int(raw_input())
print factorial(n)
```
