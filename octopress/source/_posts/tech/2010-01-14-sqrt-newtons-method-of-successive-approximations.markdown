---
layout: post_tech
title: "Sqrt Newton's Method of Successive Approximations"
date: 2010-01-14 00:00:14 +0800
comments: true
categories: [tech]
tags: [algorithm]
---


<img src="http://media-cache-ec0.pinimg.com/736x/4d/5d/8a/4d5d8aeb52644182af61a7eac04cdfc9.jpg" />

### Algorithm

```
    1. Guess

    2. Good-enough?
                      | Guess^2 - Radicant | < Predetermined Tolerance

    If yes, Guess = Root
       else, Improve Guess

    3. Improve
                      Quotient = Radicant / Guess
                      Average  = ( Guess + Quotient ) / 2
                      Guess    = Average
```

Importance: Radicant -> Large/Small -> very small fraction of the Guess -> STOP?

```perl
(define (sqrt x)
  (sqrt-iter 1.0 x))
(define (sqrt-iter guess x)
  (if (good-enough? guess x)
      guess
      (sqrt-iter (improve guess x) x)))
(define (good-enough? guess x)
  (< (abs (- (square guess) x)) 0.001))
(define (improve guess x)
  (average guess (/ x guess)))
```

Block Structure

```perl
(define (sqrt x)
  (define (good-enough? guess x)
    (< (abs (- (square guess) x)) 0.001))
  (define (improve guess x)
    (average guess (/ x guess)))
  (define (sqrt-iter guess x)
    (if (good-enough? guess x)
        guess
        (sqrt-iter (improve guess x) x)))
  (sqrt-iter 1.0 x))
```

References:  
-- [Square Roots by Newton's Method](http://mitpress.mit.edu/sicp/full-text/book/book-Z-H-10.html#%_sec_1.1.7)  
-- [Structure and Interpretation of Computer Programs](http://mitpress.mit.edu/sicp/)
