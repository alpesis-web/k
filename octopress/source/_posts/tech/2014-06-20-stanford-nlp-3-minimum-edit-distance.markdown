---
layout: post_tech
title: "Stanford NLP 3 - Minimum Edit Distance"
date: 2014-06-20 12:57:12 +0800
comments: true
categories: [tech]
tags: [nlp, machine learning]
---

## 1. Definition of Minimum Edit Distance

### 1.1. How similar are two strings?

- spell correction
    - the user typed "graffe", which is closest?
        - graf
        - graft
        - grail
        - giraffe
- computational biology
    - align two sequences of nucleotides

        `AGGCTATCACCTGACCTCCAGGCCGATGCCC`  
        `TAGCTATCACGACCGCGGTCGATTTGCCGAC`

    - resulting alignment:

        `-AGGCTATCACCTGACCTCCAGGCCGA--TGCCC---`  
        `TAG-CTATCAC--GACCGC--GGTCGATTTGCCCGAC`

- also for machine translation, information extraction, speech recognition

### 1.2. Edit Distance

- the minimum edit distance between two strings
- is the minimum number of editing operations
    - insertion
    - deletion
    - substitution
- needed to transform one into the other

### 1.3. Minimum Edit Distance

- two strings and their alignment

```
I N T E * N T I O N 
| | | | | | | | | |  
* E X E C U T I O N  
d s s   i s        

- d: deletion
- s: substitution
- i: insertion
```

- if each operation has cost of 1
    - distance between these is 5
- if substituions cost 2 (Levenshtein)
    - distance between them is 8

### 1.4. Alignment in Computational Biology

- given a sequence of bases

    `AGGCTATCACCTGACCTCCAGGCCGATGCCC`  
    `TAGCTAGCACGACCGCGGTCGATTTGCCCGAC`

- an alignment:

```
-AGGCTATCACCTGACCTCCAGGCCGA--TGCCC---
TAG-CTAGCAC--GACCGC--GGTCGATTTGCCCGAC
```

- given two sequences, alignn each letter to a letter or gap

### 1.5. Other uese of Edit Distance in NLP

- evaluating machine translation and speech recognition

```
R Spokesman confirms     senior government adviser was shot
H Spokesman said     the senior            adviser was shot dead
             S        I             D                        I
```

- named entity extraction and entity coreference
    - `IBM Inc.` announced today
    - `IBM` profits
    - `Stanford President John Hennessy` announnced yesterday
    - for `Stanford University President John Hennessy`

### 1.6. How to find the Min Edit Distance?

- searching for a path (sequence of edits) from the start string to the final string
    - initial state: the word we're tranforming
    - operators: insert, delte, substitute
    - goal state: the word we're trying to get to
    - path cost: what we want to minimize - the number of edits

```
    intention
       |---- Del ---> ntention
       |---- Ins ---> eintention
       |---- Sub ---> entention
```

### 1.7. Minimum Edit as Search

- but the space of all edit sequences is huge
    - we can't afford to navigate naively
    - lots of distinct paths wind up at the same state
        - we don't have to keep track of all of them
        - just the shortest path to each of those revisted states

### 1.8. Defining Min Edit Distance

- for two strings
    - X of length n
    - Y of length m
- we define D(i,j)
    - the edit distance between X[1..i] and Y[1..j]
        - i.e., the first i characters of X and the first j characters of Y
    - the edit distance between X and Y is thus D(n,m)

## 2. Computing Minimum Edit Distance

### 2.1. Dynamic Programming for Minimum Edit Distance

- dynamic programming: a tabular computation of D(n, m)
- solving problems by combining solutions to subproblems
- bottom-up
    - we compute D(i,j) for small i,j
    - and compute larger D(i,j) based on previously computed smaller values
    - i.e., compute D(i,j) for all i(0 < i < n) and j(0 < j < m)

### 2.2. Defining Min Edit Distance (Levenshtein)

- initialization
    - D(i,0) = i
    - D(0,j) = j
- recurrence relation:

```
for each i = 1...M
    for each j = 1...N
        D(i,j) = min { D(i-1,j) + 1  <- delete
                     { D(i,j-1) + 1  <- insert
                     { D(i-1,j-1) + 2; if X(i) <> Y(j)  <- substitute
                                    0; if X(i) == Y(j)
```

- termination
    - D(N,M) is distance

### 2.3. The Edit Distance Table

- initialization

|   |   |   |   |   |   |   |   |   |   |   |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| N | 9 |   |   |   |   |   |   |   |   |   |
| O | 8 |   |   |   |   |   |   |   |   |   |
| I | 7 |   |   |   |   |   |   |   |   |   |
| T | 6 |   |   |   |   |   |   |   |   |   |
| N | 5 |   |   |   |   |   |   |   |   |   |
| E | 4 |   |   |   |   |   |   |   |   |   |
| T | 3 |   |   |   |   |   |   |   |   |   |
| N | 2 |   |   |   |   |   |   |   |   |   |
| I | 1 |   |   |   |   |   |   |   |   |   |
| # | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|   | # | E | X | E | C | U | T | I | O | N |

- recurrence relation

|   |   |   |   |    |    |    |    |    |    |    |
|:-:|:-:|:-:|:-:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| N | 9 | 8 | 9 | 10 | 11 | 12 | 11 | 10 | 9  | 8  |  <- L = 8
| O | 8 | 7 | 8 | 9  | 10 | 11 | 10 | 9  | 8  | 9  |
| I | 7 | 6 | 7 | 8  | 9  | 10 | 9  | 8  | 9  | 10 |
| T | 6 | 5 | 6 | 7  | 8  | 9  | 8  | 9  | 10 | 11 |
| N | 5 | 4 | 5 | 6  | 7  | 8  | 9  | 10 | 11 | 10 |
| E | 4 | 3 | 4 | 5  | 6  | 7  | 8  | 9  | 10 | 9  |
| T | 3 | 4 | 5 | 6  | 7  | 8  | 7  | 8  | 9  | 8  |
| N | 2 | 3 | 4 | 5  | 6  | 7  | 8  | 7  | 8  | 7  |
| I | 1 | 2 | 3 | 4  | 5  | 6  | 7  | 6  | 7  | 8  |
| # | 0 | 1 | 2 | 3  | 4  | 5  | 6  | 7  | 8  | 9  |
|   | # | E | X | E  | C  | U  | T  | I  | O  | N  |

## 3. Bactrace for Computing Alignments

### 3.1. Computing alignments

- edit distance isn't sufficient
    - we often need to align each character of the two strings to each other
- we do this by keeping a "backtrace"
- every time we enter a cell, remember where we came from
- when we each the end,
    - trace back the path from the upper right corner to read off the alignment

### 3.2. MinEdit with Backtrace

|   |   |              |              |               |               |               |               |               |               |           |
|:-:|:-:|:------------:|:------------:|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|:---------:|
| N | 9 | (S)        8 | (SW, W, S) 9 | (SW, W, S) 10 | (SW, W, S) 11 | (SW, W, S) 12 | (S)        11 | (S)        10 | (S)        9  | (SW)   8  |
| O | 8 | (S)        7 | (SW, W, S) 8 | (SW, W, S) 9  | (SW, W, S) 10 | (SW, W, S) 11 | (S)        10 | (S)        9  | (SW)       8  | (W)    9  |
| I | 7 | (S)        6 | (SW, W, S) 7 | (SW, W, S) 8  | (SW, W, S) 9  | (SW, W, S) 10 | (S)        9  | (SW)       8  | (W)        9  | (W)    10 |
| T | 6 | (S)        5 | (SW, W, S) 6 | (SW, W, S) 7  | (SW, W, S) 8  | (SW, W, S) 9  | (SW)       8  | (W)        9  | (W)        10 | (W, S) 11 |
| N | 5 | (S)        4 | (SW, W, S) 5 | (SW, W, S) 6  | (SW, W, S) 7  | (SW, W, S) 8  | (SW, W, S) 9  | (SW, W, S) 10 | (SW, W, S) 11 | (S)    9  |
| T | 3 | (SW, W, S) 4 | (SW, W, S) 5 | (SW, W, S) 6  | (SW, W, S) 7  | (SW, W, S) 8  | (SW)       7  | (W, S)     8  | (SW, W, S) 9  | (S)    8  |
| N | 2 | (SW, W, S) 3 | (SW, W, S) 4 | (SW, W, S) 5  | (SW, W, S) 6  | (SW, W, S) 7  | (SW, W, S) 8  | (S)        7  | (SW, W, S) 8  | (SW)   7  |
| I | 1 | (SW, W, S) 2 | (SW, W, S) 3 | (SW, W, S) 4  | (SW, W, S) 5  | (SW, W, S) 6  | (SW, W, S) 7  | (SW)       6  | (W)        7  | (W)    8  |
| # | 0 | 1            | 2            | 3             | 4             | 5             | 6             | 7             | 8             | 9         |
|   | # | E            | X            | E             | C             | U             | T             | I             | O             | N         |

- S: South for down
- W: West for left
- SW: South West


### 3.3. Adding Backtrace to Minimum Edit Distance

- base conditions
    - D(i,0) = i
    - D(0,j) - j
- recurrence relation

```
for each i = 1...M
    for each j = 1...N
        D(i,j) = min { D(i-1,j) + 1  <- deletion
                     { D(i,j-1) + 1  <- insertion
                     { D(i-1,j-1) + 2; if X(i) <> Y(j)  <- substitution
                                    0; if X(i) == Y(j)

        ptr(i,j) = { LEFT  <- insertion
                   { DOWN  <- deletion
                   { DIAG  <- substitution
```

- termination:
    - D(N,M) is distance

### 3.4. The Distance Matrix

- every non-decreasing path
- from (0,0) to (M,N)
- corresponds to an alignment of the two sequences
- an optimal alignment is composed of optimal subalignments

|       |    |    |     |     |     |     |     |     |       |
|:-----:|:--:|:--:|:---:|:---:|:---:|:---:|:---:|:---:|:-----:|
| Xn    |    |    |     |     |     |     |     |     | (M,N) |
| ...   |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| x1    |    |    |     |     |     |     |     |     |       |
| x0    |    |    |     |     |     |     |     |     |       |
| (0,0) | y0 | y1 | ... | ... | ... | ... | ... | ... | Ym    |

### 3.5. Result of Backtrace

- two strings and their alignment

### 3.6. Performance

- time: O(nm)
- space: O(nm)
- backtrace: O (n+m)

## 4. Weighted Minimum Edit Distance

### 4.1. Weighted Edit Distance

- why would we add weights to the computation?
    - spell correction: some letters are more likely to be mistyped than others
    - biology: certain kinds of deletions or insertions are more likely than others

### 4.2. Confusion matrix for spelling errors

sub[X,Y] = substitution of X(incorrect) for Y(correct)  
<img src="http://media-cache-ec0.pinimg.com/736x/51/99/a9/5199a999530b83f37b93593bbddb03f0.jpg"/>

### 4.3. Weighted Min Edit Distance

- initialization
    - D(0,0) = 0
    - D(i,0) = D(i-1,0) + del[x(i)];  1 < i <= N
    - D(0,j) = D(0,j-1) + ins[y(j)];  1 < j <= M
- recurrence relation:

```
D(i,j) = min { D(i-1,j)    + del[x(i)]
             { D(i,j-1)    + ins[y(j)]
             { D(i-1,j-1)  + sub[x(i),y(j)]
```

- termination:
    - D(N,M) is distance

### 4.4. Where did the name, dynamic programming, come from?

- Richard Bellman, "Eye of the Hurricane: an autobiography" 1984

        ... The 1950s were not good years for mathematical research. [the] Secretary of Defense ... had a pathological fear and hatred of the word, research ...
        
        I decided therefore to use the word, "programming".
    
        I wanted to get across the idea that this was dynamic, this was multistage... I thought, let's ... take a word that  has an absolutely precise meaning, namely dynamic... it's impossible to use the word, dynamic, in a pejorative sense. Try thinking of some combination that will possibly give it a pejorative meaning. It's impossible.
    
        Thus, I thought dynamic programming was a good name. It was something not even a Congressman could object to.


## 5. Minimum Edit Distance in Computational Biology

### 5.1. Sequence Alignment

```
AGGCTATCACCTGACCTCCAGGCCGATGCCC
TAGCTATCACGACCGCGGTCGATTTGCCCGAC

-AGGCTATCACCTGACCTCCAGGCCGA--TGCCC---
TAG-CTAGCAC--GACCGC--GGTCGATTTGCCCGAC
```

### 5.2. Why sequence alignment?

- comparing genes or regions from different species
    - to find important regions
    - determine function
    - uncover evolutionary forces
- assembling fragments to sequence DNA
- compare individuals to looking for mutations

### 5.3. Alignments in two fields

| NLP                  | Computational Biology  |
|:---------------------|:-----------------------|
| distance (minimized) | similarity (maximized) |
| weights              | scores                 |

### 5.4. The Needleman-Wunsch Algorithm

- initialization:
    - D(i,0) = -i * d
    - D(0,j) = -j * d
- recurrence relation:

```
D(i,j) = max { D(i-1,j)   - d
             { D(i,j-1)   - d
             { D(i-1,j-1) + s[x(i),y(j)]
```

- termination:
    - D(N,M) is distance

### 5.5. The Needleman-Wunsch Matrix

- note that the origin is at the upper left

|       |    |    |     |     |     |     |     |     |       |
|:-----:|:--:|:--:|:---:|:---:|:---:|:---:|:---:|:---:|:-----:|
|       | X1 | X2 | ... | ... | ... | ... | ... | ... | Xm    |
| Y1    |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| Yn    |    |    |     |     |     |     |     |     |       |


### 5.6. A variant of the basic algorithm

- maybe it is ok to have an unlimited \# of gaps in the beginning and end

```
----------CTATCACCTGACCTCCAGGCCGATGCCCCTTCCGGC
GCGAGTTCATCTATCAC--GACCGC--GGTCG--------------
```

- if so, we don't want to penalize gaps at the ends

### 5.7. Different types of overlaps

2 overlapping "reads" from a sequencing project

```
----------||||||||||||||
          ||||||||||||||-------------------------

          ||||||||||||||-------------------------
----------||||||||||||||
```

search for a moouse gene withinn a human chromosome

```
-----------|||||||||-----------------
           |||||||||

           |||||||||
-----------|||||||||-----------------
```

### 5.8. The Overlap Detection variant

changes:

- initialization
    - for all i,j,
        - F(i,0) = 0  <-- - i * d
        - F(0,j) = 0  <-- - j * d
- termination

```
Fopt = max { max_i F(i, N)
           { max_j F(M, j)
```

|       |    |    |     |     |     |     |     |     |       |
|:-----:|:--:|:--:|:---:|:---:|:---:|:---:|:---:|:---:|:-----:|
|       | X1 | X2 | ... | ... | ... | ... | ... | ... | Xm    |
| Y1    |    |    |     |  1  |     |     |     |     |       |
| ...   |    |    |     |     |  1  |     |     |     |       |
| ...   |    |    |     |     |     |  1  |  1  |     |       |
| ...   |    |    |     |     |     |     |     |  1  |       |
| ...   |    |    |     |     |     |     |     |  1  |       |
| ...   |    |    |     |     |     |     |     |     |   1   |
| ...   |    |    |     |     |     |     |     |     |       |
| ...   |    |    |     |     |     |     |     |     |       |
| Yn    |    |    |     |     |     |     |     |     |       |


### 5.9. The Local Alignment Problem

- given two strings
    - x = x1...xm
    - y = y1...yn
- find substrings x', y' whose similarity (optimal global alignment value) is maximum

    x = aaaacc`cccggg`gtta  
    y = tt`cccggg`aaccaacc

### 5.10. The Simith-Waterman algorithm

idea: Ignore badly aligning regions

modifications to Needleman-Wunsch:

- initialization: 
    - F(0,j) = 0
    - F(i,0) = 0
- iteration

```
F(i,j) = max { F(i-1,j)    - d
             { F(i,j-1)    - d
             { F(i-1, j-1) + s(xi,yj)
```

- termination
    - if we want the best local alignment
        - Fopt = max_i,j F(i,j)
        - find Fopt and trance back
    - if we want all local alignments scoring > t
        - for all i,j find F(i,j) > t, and trace back?
        - complicated by overlapping local alignments

### 5.11. Local alignment example

X = `ATCAT`  
Y = `ATTAT`C

X =    `ATC`AT  
Y = ATT`ATC`  

let:  
- m = 1 (1 point for match)
- d = 1 (-1 point for del/ins/sub)

|   |   | A | T | T | A | T | C |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|   | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| A | 0 | 1 | 0 | 0 | 1 | 0 | 0 |
| T | 0 | 0 | 2 | 1 | 0 | 2 | 0 |
| C | 0 | 0 | 1 | 1 | 0 | 1 | 3 |
| A | 0 | 1 | 0 | 0 | 2 | 1 | 2 |
| T | 0 | 0 | 2 | 0 | 1 | 3 | 2 |


## References

1. video courses: [part 1][course-1], [part 2][course-2], [part 3][course-3], [part 4][course-4], [part 5][course-5]

  [course-1]: https://www.youtube.com/watch?v=CXfJNzD43OI&index=7&list=PL6397E4B26D00A269
  [course-2]: https://www.youtube.com/watch?v=z_CB7Gih_Mg&list=PL6397E4B26D00A269&index=8 
  [course-3]: https://www.youtube.com/watch?v=iQVp4Mq6s6k&index=9&list=PL6397E4B26D00A269 
  [course-4]: https://www.youtube.com/watch?v=ScdU0cHmxfE&index=10&list=PL6397E4B26D00A269 
  [course-5]: https://www.youtube.com/watch?v=Q0TGn4wkuoE&index=11&list=PL6397E4B26D00A269 
