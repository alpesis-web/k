---
layout: post_tech
title:  "Multiplication Tables"
date:   2014-09-02 20:10:00 +0800
comments: true
categories: [tech]
tags: [python, algorithm]
---

## Chinese 9-9 Multiplication Table

Print a multiplication table 

    1*1=1	
    1*2=2	2*2=4	
    1*3=3	2*3=6	3*3=9	
    1*4=4	2*4=8	3*4=12	4*4=16	
    1*5=5	2*5=10	3*5=15	4*5=20	5*5=25	
    1*6=6	2*6=12	3*6=18	4*6=24	5*6=30	6*6=36	
    1*7=7	2*7=14	3*7=21	4*7=28	5*7=35	6*7=42	7*7=49	
    1*8=8	2*8=16	3*8=24	4*8=32	5*8=40	6*8=48	7*8=56	8*8=64	
    1*9=9	2*9=18	3*9=27	4*9=36	5*9=45	6*9=54	7*9=63	8*9=72	9*9=81	

solution 1:

```python
    def print99():
        for base in range(1,10):
            row = ''
            for col in range(1,base+1):
                row = row + str(col) + "*" + str(base) + "=" + str(col*base) + "\t"
            print row
```

solution 2:

```python
print '\n'.join([' '.join(['%s*%s=%-2s' % (y,x,x*y) for y in range(1,x+1)]) for x in range(1,10)])
```


## Summation

Print a summation.

    1 = 1
    1 + 2 = 3
    1 + 2 + 3 = 6
    1 + 2 + 3 + 4 = 10
    1 + 2 + 3 + 4 + 5 = 15
    1 + 2 + 3 + 4 + 5 + 6 = 21
    1 + 2 + 3 + 4 + 5 + 6 + 7 = 28
    1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 = 36
    1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 = 45
    1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 10 = 55

solution:

```python
def calSum(num):
	digits = []
	digitStrings = []
	delimiter = ' + '
	
	for i in range(1,num+1):
		digits.append(i)
		digitStrings.append(str(i))
	print delimiter.join(digitStrings),"=",sum(digits)

for i in range(1,11):
	calSum(i)
```
