---
layout: post_tech
title:  "Unbalanced Coin"
date:   2014-07-29 22:54:00 +0800
comments: true
categories: [tech]
tags: [python, algorithm]
---

If there is an unbalanced coin, the probability of the head is p (0\<\p\<1), while
of the tail is 1-p, how can you get the same probability of this coin?

To flip the coin twice,
 
- 1\. prob(head, head) = p * p
- 2\. prob(head, tail) = p * (1-p)
- 3\. prob(tail, head) = (1-p) * p
- 4\. prob(tail, tail) = (1-p) * (1-p)

If the situation 1 & 4, flip the coin twice again.

```python
import random

class UnbalancedCoin:

    def __init__(self, p):

        assert 0.0 < p < 1.0, 'invalid p'
        self.p = p

    def flip(self, n):

        combination = []
        for i in range(n):
            pos = random.choice(['head', 'tail'])
            combination.append(pos)

        return combination 

    def flip_prob(self, combination):
        
        prob_flip = 1
        for item in combination:
            if item == 'head':
                prob = self.p
            else:
                prob = 1 - self.p
            prob_flip = prob_flip * prob

        return prob_flip


if __name__ == '__main__':

    p = 0.3
    n = 2
    coin = UnbalancedCoin(p)
    combination = coin.flip(n)
    prob = coin.flip_prob(combination)
    print "combination: ", combination
    print "probability: ", prob
```    

