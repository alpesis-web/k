---
layout: post_tech
title: "Programming Languages Comparison: print"
date: 2016-04-05 17:08:53 +0800
comments: true
categories: [tech]
tags: [programming, python, c, c++, java]
toc: true
---

## Task

- Save a line of input from stdin to a variable
- Print `Hello, World` on a single line
- Print the value of the variable on a second line

## Solutions

### C

```c
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>
int main() {
   char inputString[105]; // declare a variable to hold our input
   scanf("%[^\n]", inputString); // get a line of input from stdin and save it to our variable
  
   // Your first line of output goes here
   printf("Hello, World.\n");

   // Write the second line of output
   printf(inputString);

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

int main() {
   string inputString; // declare a variable to hold our input
   getline(cin, inputString); // get a line of input from cin and save it to our variable
  
   // Your first line of output goes here
   cout << "Hello, World." << endl;

   // Write the second line of output
   cout << inputString << endl;

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
      Scanner scan = new Scanner(System.in); // use the Scanner class to read from stdin
      String inputString = scan.nextLine(); // read a line of input and save it to a variable
      scan.close(); // close the scanner
      
      // Your first line of output goes here
      System.out.println("Hello, World.");
      
      // Write the second line of output
      System.out.println(inputString);
   }
}
```

### Python

python2

```python
inputString = raw_input() # get a line of input from stdin and save it to our variable

# Your first line of output goes here
print 'Hello, World.'

# Write the second line of output
print inputString
```

python3

```python
inputString = input() # get a line of input from stdin and save it to our variable

# Your first line of output goes here
print('Hello, World.')

# Write the second line of output
print(inputString)
```
