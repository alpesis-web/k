---
layout: post_tech
title: "Programming Languages Comparison: Operators"
date: 2016-04-07 23:57:09 +0800
comments: true
categories: [tech]
tags: [python, c, c++, java, programming]
toc: true
---

## Task

- get the input: mealCost, tipPercent, taxPercent
- calculate the tip: tip = mealCost * tipPercent / 100
- calculate the tax: tax = mealCost * taxPercent / 100
- calculate the totalCost: totalCost = mealCost + tip + tax
- round the totalCost
- print the output


## Solutions

### C

```c
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

int main() {

    /* Enter your code here. Read input from STDIN. Print output to STDOUT */
    float mealCost;
    float tipPercent;
    float taxPercent;
    
    scanf("%f", &mealCost);
    scanf("\n%f\n", &tipPercent);
    scanf("%f", &taxPercent);
    
    double tip = mealCost * tipPercent / 100.0;
    double tax = mealCost * taxPercent / 100.0;
    int totalCost = round(mealCost + tip + tax);
    
    printf("The total meal cost is %d dollars.", totalCost);
    
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
    /* Enter your code here. Read input from STDIN. Print output to STDOUT */ 
    double mealCost;
    double tipPercent;
    double taxPercent;
    
    cin >> mealCost;
    cin >> tipPercent;
    cin >> taxPercent;
    
    double tip = mealCost * tipPercent / 100;
    double tax = mealCost * taxPercent / 100;
    double totalCost = mealCost + tip + tax;
    cout << "The total meal cost is " << round(totalCost) << " dollars." << endl;
       
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
public class Arithmetic {

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        double mealCost = scan.nextDouble(); // original meal price
        double tipPercent = scan.nextInt(); // tip percentage
        double taxPercent = scan.nextInt(); // tax percentage
        scan.close();
      
        // Write your calculation code here.
        double tip = mealCost * (tipPercent/100);
        double tax = mealCost * (taxPercent/100);
      
        // cast the result of the rounding operation to an int and save it as totalCost
        int totalCost = (int) Math.round(mealCost+tip+tax);
      
        // Print your result
        System.out.println("The total meal cost is " + totalCost + " dollars.");
    }
}
```

###Python

Python 2

```python
mealCost = float(raw_input())
tipPercent = float(raw_input())
taxPercent = float(raw_input())

tip = mealCost * ( tipPercent/100)
tax = mealCost * (taxPercent/100)

totalCost = round(mealCost + tip + tax)

print "The total meal cost is %d dollars." % totalCost
```

Python 3

```python
mealCost = float(input())
tipPercent = float(input())
taxPercent = float(input())

tip = mealCost * (tipPercent/100)
tax = mealCost * (taxPercent/100)

totalCost = round(mealCost + tip + tax)
print("The total meal cost is %d dollars." % totalCost)
```
