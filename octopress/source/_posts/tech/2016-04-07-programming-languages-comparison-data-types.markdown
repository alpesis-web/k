---
layout: post_tech
title: "Programming Languages Comparison: Data Types"
date: 2016-04-07 01:55:32 +0800
comments: true
categories: [tech]
tags: [c, c++, python, java, programming]
toc: true
---

## Task

- get the variables: int, double, string
- calculate the sum of the digits and concat the strings
- print the results

## Solutions

### C

```c
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

int main() {
    int i = 4;
    double d = 4.0;
    char s[] = "HackerRank ";
    
    // Declare second integer, double, and String variables.
    int num;
    float digit;
    char string[100];
    
    // Read and save an integer, double, and String to your variables.
    scanf("%d", &num);
    scanf("\n%f\n", &digit);
    scanf("%[^\n]", string);

    // Print the sum of both integer variables on a new line.
    printf("%d\n", i+num);    

    // Print the sum of the double variables on a new line.
    printf("%.1f\n", d+digit);    

    // Concatenate and print the String variables on a new line
    // The 's' variable above should be printed first.
    printf("%s%s", s, string);

    return 0;
}
```

### C++

```cpp
#include <iostream>
#include <iomanip>
#include <limits>

using namespace std;

int main() {
    int i = 4;
    double d = 4.0;
    string s = "HackerRank ";

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
        int i = 4;
        double d = 4.0;
        String s = "HackerRank ";
		
        Scanner scan = new Scanner(System.in);

        /* Declare second integer, double, and String variables. */
        int i2 = scan.nextInt();
        double d2 = scan.nextDouble();
        String empty = scan.nextLine();
        String s2 = scan.nextLine();

        /* Read and save an integer, double, and String to your variables.*/
        int int_sum = i + i2;
        double double_sum = d + d2;
        String combine_s = s + s2;

        /* Print the sum of both integer variables on a new line. */
        System.out.println(int_sum);
        /* Print the sum of the double variables on a new line. */
        System.out.println(double_sum);
        /* Concatenate and print the String variables on a new line; 
        	the 's' variable above should be printed first. */
        System.out.println(combine_s);

        scan.close();
    }
}
```

### Python

python 2

```python
i = 4
d = 4.0
s = 'HackerRank '

# Declare second integer, double, and String variables.
i2 = int(raw_input())
d2 = float(raw_input())
s2 = str(raw_input())

# Read and save an integer, double, and String to your variables.
total_i = i + i2
total_d = d + d2
strings = s + s2

# Print the sum of both integer variables on a new line.
print str(total_i)

# Print the sum of the double variables on a new line.
print str(total_d)

# Concatenate and print the String variables on a new line
# The 's' variable above should be printed first.
print strings
```

python 3

```
i = 4
d = 4.0
s = 'HackerRank '

# Declare second integer, double, and String variables.
i2 = int(input())
d2 = float(input())
s2 = str(input())

# Read and save an integer, double, and String to your variables.
total_i = i + i2
total_d = d + d2
strings = s + s2

# Print the sum of both integer variables on a new line.
print(total_i)

# Print the sum of the double variables on a new line.
print(total_d)

# Concatenate and print the String variables on a new line
# The 's' variable above should be printed first.
print(strings)
```
