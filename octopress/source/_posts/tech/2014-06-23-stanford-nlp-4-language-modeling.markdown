---
layout: post_tech
title:  "Stanford NLP 4 - Language Modeling"
date:   2014-06-23 13:49:23 +0800
comments: true
categories: [tech]
tags: [nlp, machine learning]
---

## 1. Introduction to N-grams

### 1.1. Probabilistic Language Models

- goal: assign a probability to a sentence
    - mahcine translation: $$ P(high\,winds\,tonite) > P(large\,winds\,tonite) $$
    - spell correction: $$P(about\,fifteen\,minutes\,from) > P(about\,fifteen\,minuets\,from)$$
    - speech recognition: $$P(I\,saw\,a\,van) >> P(eyes\,awe\,of\,an)$$
    - summarization, question-answering, etc., etc.!!
- goal: compute the probability of a sentence or sequence of words $$P(W) = P(w1,w2,w3,w4,w5...wn)$$
- related task: probability of an upcoming word $$P(w5\|w1,w2,w3,w4)$$
- a model that computes either of these $$P(W)$$ or $$P(Wn\|w1,w2...wn-1)$$ is called a language model
- better: the grammar, but language model or LM is standard

### 1.2. How to compute P(W)

- how to compute this joint probability: $$P(its, water, is, so transparent, that)$$
- intuition: let's rely on the Chian Rule of Probability

### 1.3. Reminder: The Chain Rule

- recall the definition of conditional probabilities  
    $$P(A\|B) = P(A,B) / P(B)$$  
    $$P(A\|B)*P(B) = P(A,B)$$  
    $$P(A,B) = P(A\|B)*P(B)$$  
- more variables:  
    $$P(A,B,C,D) = P(A)P(B\|A)P(C\|A,B)P(D\|A,B,C)$$  
    the chain rule in general: $$P(x1,x2,x3,...,xn) = P(x1)P(x2\|x1)P(x3\|x1,x2)...P(xn\|x1,...,xn-1)$$  

### 1.4. The Chain Rule applied to compute joint probability of words in sentence

$$P(w_1*w_2...w_n = \prod_i {P(w_i \| w_1 * w_2...w_{i-1})}$$

$$P("its water is so transparent") = P(its) * P(water\|its) * P(is\| its\,water) * P(so\| its\,water\,is) * P(transparent\| its\,water\,is\,so)$$


### 1.5. How to estimate these probabilities

- could we just count and divide?

  $$P(the\| its\,water\,is\,so\,transparent\,that) = Count(its\,water\,is\,so\,transparent\,that\,the) / Count(its\,water\,is\,so\,transparent\,that)$$

- NO! TOO MANY possible sentences!
- we'll never see enough data for estimating these

### 1.6. Markov Assumption

- simplifying assumption  
  $$P(the\|its\,water\,is\,so\,transparent\,that) \approx P(the\| that)$$
- or maybe  
  $$P(the\|its\,water\,is\,so\,transparent\,that) \approx P(the\|transparent\,that)$$

### 1.7. Markov Assumption

the probability of current word = the conditional probability of some previous words

$$P(w_1w_2...w_n) \approx \prod_i P(w_i \| w_{i-k}...w_{i-1})$$

- in other words, we approximate each component in the product

$$P(w_i \| w_1w_2...w_{i-1}) \approx P(w_i \| w_{i-k}...w_{i-1})$$


### 1.8. Simplest case: unigram model

the probability of currect = the probability of the previous word

$$P(w_1w_2...w_n) \approx \prod_i P(w_i)$$

### 1.9. N-gram models

- we can extend to trigrams, 4-grams, 5-grams
- in general this is an insufficient model of language
    - because language has long-distance dependencies:

"The (computer) which I had just put into the machine room on the fifth (floor) [crashed]"

- but we can often get away with N-gram models


## 2. Estimating N-gram Probabilities

### 2.1. Estimating bigram probabilities

- the maximum likelihood estimate

$$P(w_i \| w_{i-1} = count(w_{i-1},w_i) / count(w_{i-1})$$  
$$P(w_i \| w_{i-1} = c(w_{i-1}, w_i) / c(w_{i-1})$$

### 2.2. An example

$$ P(w_i \| w_{i-1}) = c(w_{i-1},w_i) / c(w_{i-1}) $$

```
    <s> I am Sam </s>  
    <s> Sam I am </s>  
    <s> I do not like green eggs and ham </s>  
```

- $$ P(I | `<s>`) = c(`<s>`,I) / c(`<s>`) = 2/3 = .67 $$  
- $$ P(Sam | `<s>`) = 1/3 = .33 $$  
- $$ P(am | I) = 2/3 = .67 $$  
- $$ P(</s> | Sam) = 1/2 = 0.5 $$  
- $$ P(Sam | am) = 1/2 = .5 $$  
- $$ P(do | I) = 1/3 = .33 $$  

### 2.3. More examples: Berkeley Restaurant Project sentences

- can you tell me about any good cantonese restaurants close by 
- mid priced thai food is what i'm looking for
- tell me about chez panisse
- can you give me a listing of the kinds of food that are available
- i'm looking for a good place to eat breakfast
- when is caffe venezia open during the day

### 2.4. Raw bigram counts

- out of 9222 sentences

$$P(w_i \| w_{i-1}) = c(w_{i-1}, w_i) / c(w_{i-1})$$

|         | i  | want | to  | eat | chinese | food | lunch | spend |
|:--------|:---|:-----|:----|:----|:--------|:-----|:------|:------|
| i       | 5  | 827  | 0   | 9   | 0       | 0    | 0     | 2     |
| want    | 2  | 0    | 608 | 1   | 6       | 6    | 5     | 1     |
| to      | 2  | 0    | 4   | 686 | 2       | 0    | 6     | 211   |
| eat     | 0  | 0    | 2   | 0   | 16      | 2    | 42    | 0     |
| chinese | 1  | 0    | 0   | 0   | 0       | 82   | 1     | 0     |
| food    | 15 | 0    | 15  | 0   | 1       | 4    | 0     | 0     |
| lunch   | 2  | 0    | 0   | 0   | 0       | 1    | 0     | 0     |
| spend   | 1  | 0    | 1   | 0   | 0       | 0    | 0     | 0     | 

- normalize by unigrams

| i    | want | to   | eat  | chinese | food | lunch | spend |
|:-----|:-----|:-----|:-----|:--------|:-----|:------|:------|
| 2533 | 927  | 2417 | 746  | 158     | 1093 | 341   | 278   |

- result

|         | i       | want | to     | eat    | chinese | food   | lunch  | spend   |
|:--------|:--------|:-----|:-------|:-------|:--------|:-------|:-------|:--------|
| i       | 0.002   | 0.33 | 0      | 0.0036 | 0       | 0      | 0      | 0.00079 |
| want    | 0.0022  | 0    | 0.66   | 0.0011 | 0.0065  | 0.0065 | 0.0054 | 0.0011  |
| to      | 0.00083 | 0    | 0.0017 | 0.28   | 0.00083 | 0      | 0.0025 | 0.087   |
| eat     | 0       | 0    | 0.0027 | 0      | 0.021   | 0.0027 | 0.056  | 0       |
| chinese | 0.0063  | 0    | 0      | 0      | 0       | 0.52   | 0.0063 | 0       |
| food    | 0.014   | 0    | 0.014  | 0      | 0.00092 | 0.0037 | 0      | 0       |
| lunch   | 0.0059  | 0    | 0      | 0      | 0       | 0.0029 | 0      | 0       |
| spend   | 0.0036  | 0    | 0.0036 | 0      | 0       | 0      | 0      | 0       | 


### 2.5. Bigram estimates of sentence probabilities

```
    P (<s> I want english food </s>) = P(I | <s>) *
                                       P(want | I) *
                                       P(english | want) *
                                       P(food | english) *
                                       P(</s> | food)
                                     = 0.000031
```

### 2.6. What kinds of knowledge?

```
    P(english | want) = 0.0011
    P(chinese | want) = 0.0065   <- people like chinese cuisine
    P(to | want) = 0.66          <- want - infinite grammar
    P(eat | to) = 0.28
    P(food | to) = 0
    P(want | spend) = 0          <- grammatical disallowed
    P(i | <s>) = 0.25
```

### 2.7. Practical Issues

- what do everything in log space
    - avoid underflow
    - also adding is faster than multiplying

$$p_1 * p_2 * p_3 * p_4 = log p_1 + log p_2 + log p_3 + log p_4$$


### 2.8. Language Modeling Toolkits

- [SRILM](http://www.speech.sri.com/projects/srilm/)
- [Google N-Gram Release, August 2006](http://googleresearch.blogspot.ae/2006/08/all-our-n-gram-are-belong-to-you.html)
- [Google Book N-grams](http://ngrams.googlelabs.com/)


## 3. Evaluation and Perplexity

### 3.1. Evaluation: How good is our model?

- does our language model prefer good sentences to bad ones?
    - assign higher probability to "real" or "frequently observed" sentences
        - than "ungrammatical" or "rarely observed" sentences?
- we train parameters of our model on a training set
- we test the model's performance on data we haven't seen
    - a test set is an unseen dataset that is different from our training set, totally unused
    - an evaluation metric tells us how well our model does on the test set

### 3.2. Extrinsic evaluation of N-gram models

- best evaluation for comparing models A and B
    - put each model in a task
        - spelling corrector, speech recognizer, MT system
    - run the task, get an accuracy for A and for B
        - how many misspelled words corrected properly
        - how many words translated correctly
    - compare accuracy for A and B


### 3.3. Difficulty of extrinsic (in-vivo) evaluation of N-gram models

- extrinsic evaluation
    - time-consuming, can tak days or weeks
- so
    - sometimes use intrinsic evaluation: perplexity
    - bad approximation
        - unless the test data looks just like the training data
        - so generally only useful in pilot experiments
    - but is helpful to think about

### 3.4. Intuition of Perplexity

- the Shannon Game:
    - How well can we predict the next word?

```
          I always order pizza with cheese and ____ 
          - mushrooms 0.1
          - pepperoni 0.1
          - anchovies 0.01
          - fried rice 0.0001
          - ...
          - and 1e-100
        
          The 33rd President of the US was _____
          I saw a ______
```
    
    - unigrams are terrible at this game (why?)
- a better model of a text
    - is one which assigns a higher probability to the word that actually occurs

### 3.5. Perplexity

the best language model is one that best predicts an unseen test set  
- gives the highest $$P(sentence)$$

Perplexity is the probability of the test test, normalized by the number of words  
$$PP(W) = P(w_1w_2...w_N)^{-1/N} = \sqrt[N]{1/P(w_1w_2...w_N)}$$

Chain rule:  
$$PP(W) = \sqrt[N]{\prod_{i=1}^N 1 / P(w_i\|w_1...w_{i-1})}$$

for bigrams:  
$$PP(W) = \sqrt[N]{\prod_{i=1}^N 1 / P(w_i \| w_{i-1})}$$

minimizing perplexity is the same as maximmizing probability

### 3.6. The Shannon Game intuition for perplexity

- from Josh Goodman
- How hard is the task of recognizing digits '0,1,2,3,4,5,6,7,8,9'
    - perplexity 10
- How hard is recognizing (30,000) names at Microsoft
    - perplexity = 30,000
- if a system has to recognize
    - operator (1 in 4)
    - sales (1 in 4)
    - technical support (1 in 4)
    - 30,000 names (1 in 120,000 each)
    - perplexity is 54
- perplexity is weighted equivalent branching factor

### 3.7. Perplexity as branching factor

- let's suppose a sentence consisting of random digits
- what is the perplexity of this sentence according to a model that assign P=1/20 to each digit?

$$PP(W) = P(w_1w_2...w_N)^{-1/N} = (1^N/10)^{-1/N} = 1^(-1)/10 = 10$$

### 3.8. Lower perplexity = better model

- training 38 million words, test 1.5 million words, WSJ

| N-gram Order | Unigram | Bigram | Trigram |
|:-------------|:--------|:-------|:--------|
| Perplexity   | 962     | 170    | 109     |

## 4. Generalization and Zeros

### 4.1. The Shannon Visualization Method

- choose a random bigram
    - (<s>,w) according to its probability
- now choose a random bigram
    - (w,x) according to its probability
- and so on until we choose </s>
- then string the words together

### 4.2. Shakespeare as corpus

- N=884,647 tokens, V=29,066
- Shakespeare produced 300,000 bigram types out of V^2=844 million possible bigrams
    - so 99.96% of the possible bigrams were never seen (have zero entries in the table)
- Quadrigrams worse: what's coming out looks like Shakespeare because it is Shakespeare

### 4.3. The perils of overfitting

- N-grams only work well for word prediction if the test corpus looks like the training corpus
    - in real life, it often doesn't
    - we need to train robust models that generalize!
    - one kind of generalization: Zeros!
        - things that don't ever occur in the training set
            - but occur in the test set

### 4.4. Zero

- training set:
    - denied the allegations
    - denied the reports
    - denied the claims
    - denied the request

$$P("offer" \| denied\,the) = 0$$

- test set:
    - denied the offer
    - denied the loan

$$P("offer" \| denied\,the) = 0?$$

### 4.5. Zero probability bigrams

- bigrams with zero probability
    - mean that we will assign 0 probability to the test set
- and hence we cannot compute perplexity (can't divide by 0)!


## 5. Smoothing Add-One (Laplace) smoothing

### 5.1. The intuition of smoothing from Dan Klein)

- when we have sparse statistics
    - P(w \| denied the) 
        - 3 allegations
        - 2 reports
        - 1 claims
        - 1 request
        - 7 total
- steal probability mass to generalize better
    - P(w \| denied the)
        - 2.5 allegations
        - 1.5 reports
        - 0.5 claims
        - 0.5 request
        - 2 other
        - 7 total

### 5.2. Add-one estimation

- also called Laplace smoothing
- pretend we saw each word one more time than we did
- just add one to all the counts

MLE estimate: $$P_MLE(w_i \| w_{i-1}) = c(w_{i-1},w_i) / c(w_{i-1})$$  
Add-1 estimate: $$P_Add-1(w_i \| w_{i-1}) = \frac {c(w_{i-1},w_i) + 1}{c(w_{i-1}) + V}$$

### 5.3. Maximum Likelihood Estimates

- the maximum likelihood estimate
    - of some parameter of a model M from a training set T
    - maximizes the likelihood of the training set T given the model M
- suppose the word "bagel" occurs 400 times in a corpus of a million words
- what is the probability that a random word from some other text will be "bagel"?
- MLE estimate is 400/1,000,000 = 0.0004
- this may be a bad estimate for some other corpus
    - but it is the estimate that makes it most likely that "bagel" will occur 400 times in a million word corpus

### 5.4. Berkeley Restaurant Corpus: Laplace smoothed bigram counts

|         | i  | want | to  | eat | chinese | food | lunch | spend |
|:--------|:---|:-----|:----|:----|:--------|:-----|:------|:------|
| i       | 6  | 828  | 1   | 10  | 1       | 1    | 1     | 3     |
| want    | 3  | 1    | 609 | 2   | 7       | 7    | 6     | 2     |
| to      | 3  | 1    | 5   | 687 | 3       | 1    | 7     | 212   |
| eat     | 1  | 1    | 3   | 1   | 17      | 3    | 43    | 1     |
| chinese | 2  | 1    | 1   | 1   | 1       | 83   | 2     | 1     |
| food    | 16 | 1    | 16  | 1   | 2       | 5    | 1     | 1     |
| lunch   | 3  | 1    | 1   | 1   | 1       | 2    | 1     | 1     |
| spend   | 2  | 1    | 2   | 1   | 1       | 1    | 1     | 1     | 


### 5.5. Laplace-smoothed bigrams

$$P^*(w_n \| w_{n-1}) = \frac {C(w_{n-1}w_n) + 1}{C(w_{n-1}) + V}$$

|         | i       | want    | to      | eat     | chinese | food    | lunch   | spend   |
|:--------|:--------|:--------|:--------|:--------|:--------|:--------|:--------|:--------|
| i       | 0.00015 | 0.21    | 0.00025 | 0.0025  | 0.00025 | 0.00025 | 0.00025 | 0.00075 |
| want    | 0.0013  | 0.00042 | 0.26    | 0.00084 | 0.0029  | 0.0029  | 0.0025  | 0.00084 |
| to      | 0.00078 | 0.00026 | 0.0013  | 0.18    | 0.00078 | 0.00026 | 0.0018  | 0.055   |
| eat     | 0.00046 | 0.00046 | 0.0014  | 0.00046 | 0.078   | 0.0014  | 0.02    | 0.00046 |
| chinese | 0.0012  | 0.00062 | 0.00062 | 0.00062 | 0.00062 | 0.052   | 0.0012  | 0.00062 |
| food    | 0.0063  | 0.00039 | 0.0063  | 0.00039 | 0.00079 | 0.002   | 0.00039 | 0.00039 |
| lunch   | 0.0017  | 0.00056 | 0.00056 | 0.00056 | 0.00056 | 0.0011  | 0.00056 | 0.00056 |
| spend   | 0.0012  | 0.00058 | 0.00058 | 0.00058 | 0.00058 | 0.00058 | 0.00058 | 0.00058 | 


### 5.6. Reconstituted counts

$$C^*(w_{n-1}w_n) = \frac {[C(w_{n-1}w_n) + 1] * C(w_{n-1})}{C(w_{n-1}) + V}$$

|         | i    | want  | to    | eat   | chinese | food    | lunch   | spend   |
|:--------|:-----|:------|:------|:------|:--------|:--------|:--------|:--------|
| i       | 3.8  | 527   | 0.64  | 6.4   | 0.64    | 0.64    | 0.64    | 1.9     |
| want    | 1.2  | 0.39  | 238   | 0.78  | 2.7     | 2.7     | 2.3     | 0.78    |
| to      | 1.9  | 0.63  | 3.1   | 430   | 1.9     | 0.63    | 4.4     | 133     |
| eat     | 0.34 | 0.34  | 1     | 0.34  | 5.8     | 1       | 15      | 0.34    |
| chinese | 0.2  | 0.098 | 0.098 | 0.098 | 0.098   | 8.2     | 0.2     | 0.098   |
| food    | 6.9  | 0.43  | 6.9   | 0.43  | 0.86    | 2.2     | 0.43    | 0.43    |
| lunch   | 0.57 | 0.19  | 0.19  | 0.19  | 0.19    | 0.38    | 0.19    | 0.19    |
| spend   | 0.32 | 0.16  | 0.32  | 0.16  | 0.16    | 0.16    | 0.16    | 0.16    | 

### 5.7. Compare with raw bigram counts

|         | i  | want | to  | eat | chinese | food | lunch | spend |
|:--------|:---|:-----|:----|:----|:--------|:-----|:------|:------|
| i       | 5  | 827  | 0   | 9   | 0       | 0    | 0     | 2     |
| want    | 2  | 0    | 608 | 1   | 6       | 6    | 5     | 1     |
| to      | 2  | 0    | 4   | 686 | 2       | 0    | 6     | 211   |
| eat     | 0  | 0    | 2   | 0   | 16      | 2    | 42    | 0     |
| chinese | 1  | 0    | 0   | 0   | 0       | 82   | 1     | 0     |
| food    | 15 | 0    | 15  | 0   | 1       | 4    | 0     | 0     |
| lunch   | 2  | 0    | 0   | 0   | 0       | 1    | 0     | 0     |
| spend   | 1  | 0    | 1   | 0   | 0       | 0    | 0     | 0     | 

add-1 smoothing is maximum to change the original counts

|         | i    | want  | to    | eat   | chinese | food    | lunch   | spend   |
|:--------|:-----|:------|:------|:------|:--------|:--------|:--------|:--------|
| i       | 3.8  | 527   | 0.64  | 6.4   | 0.64    | 0.64    | 0.64    | 1.9     |
| want    | 1.2  | 0.39  | 238   | 0.78  | 2.7     | 2.7     | 2.3     | 0.78    |
| to      | 1.9  | 0.63  | 3.1   | 430   | 1.9     | 0.63    | 4.4     | 133     |
| eat     | 0.34 | 0.34  | 1     | 0.34  | 5.8     | 1       | 15      | 0.34    |
| chinese | 0.2  | 0.098 | 0.098 | 0.098 | 0.098   | 8.2     | 0.2     | 0.098   |
| food    | 6.9  | 0.43  | 6.9   | 0.43  | 0.86    | 2.2     | 0.43    | 0.43    |
| lunch   | 0.57 | 0.19  | 0.19  | 0.19  | 0.19    | 0.38    | 0.19    | 0.19    |
| spend   | 0.32 | 0.16  | 0.32  | 0.16  | 0.16    | 0.16    | 0.16    | 0.16    | 

### 5.8. Add-1 estimation is a blunt instrument

- so add-1 isn't used for N-grams
    - we'll see better methods
- but add-1 is used to smooth other NLP models
    - for text classification
    - in domains where the number of zeros isn't so huge

## 6. Interpolation, Backoff, and Web-Scale LMs

### 6.1. Backoff and Interpolation

- sometimes it helps to use less context
    - condition on less context for contexts you haven't learned much about
- backoff
    - use trigram if you have good evidence
    - otherwise bigram
    - otherwise unigram
- interpolation
    - mix unigram, bigram, trigram
- interpolation works better

### 6.2. Linear Interpolation

- simple interpolation

$$\hat P(w_n \| w_{n-1}w_{n-2}) = \lambda_1 P(w_n \| w_{n-1}w_{n-2}) + \lambda_2 P(w_n \| w_{n-1}) + \lambda_3 P(w_n)$$  
$$\sum_i \lambda_i = 1$$

- lambdas conditional on context

$$\hat P(w_n \| w_{n-2}w_{n-1}) = \lambda_1 (w_{n-2}^{n-1}) P(w_n \| w_{n-2}w_{n-1}) 
                                  + \lambda_2 (w_{n-2}^{n-1}) P(w_n \| w_{n-1})
                                  + \lambda_3 (w_{n-2}^{n-1}) P(w_n)
$$


### 6.3. How to set the lambdas

- use a held-out corpus

training data -> held-out data (dev set) -> test data

- choose $$\lambda$$s to maximize the probability of held-out data
    - fix the N-gram probabilities (on the training data)
    - then search for $$\lambda$$s that give largest probability to held-out set:

$$log P(w_1...w_n \| M(\lambda_1...\lambda_k)) = \sum_i log P_{M(\lambda_1...\lambda_k)}(w_i \| w_{i-1})$$


### 6.4. Unknown words: Open versus closed vocabulary tasks

- if we know all the words in advanced
    - vocabulary V is fixed
    - closed vcabulary task
- often we don't know this
    - out of vocabulary = OOV words
    - open vocabulary task
- instead: create an unknown word token <UNK>
    - training of <UNK> probabilities
        - create a fixed lexicon L of size V
        - at text normalization phase, any training word not in L changed to <UNK>
        - now we train its probabilities like a normal word
    - at decoding time
        - if text input: use UNK probabilities for any word not in training 

### 6.5. Smoothing for Web-scale N-grams

- "stupid backoff" (Brants et al. 2007)
- no discounting, just use relative frequencies

$$S(w_i \| w_{i-k+1}^{i-1}) = \{ \frac {count(w_{i-k+1}^i)}{count(w_{i-k+1}^{i-1})} \qquad if \quad count(w_{i=k+1}^i) > 0$$  
$$S(w_i \| w_{i-k+1}^{i-1}) = \{ 0.4S(w_i \| w_{i-k+2}^{i-1})  \qquad otherwise$$


$$S(w_i) = \frac {count(w_i)}{N}$$


### 6.6. N-gram Smoothing Summary

- Add-1 smoothing
    - OK for text categorization, not for language modeling
- the most commonly used method
    - extended interpolated Kneser-ney
- For very large N-grams like the web
    - stupid backoff

### 6.7. Advanced language Modeling

- Discriminative models
    - choose n-gram weights to improve a task, not to fit the training set
- parsing-based models
- caching models
    - recently used words are more likely to appear

        $$P_{CACHE}(w \| history) = \lambda P(w_i \| w_{i-2}w_{i-1}) + (1-\lambda) \frac{c(w\in history)}{\|history\|} $$

    - these perform very poorly for speech recognition (why?)

## 7. Advanced: Good-Turing Smoothing

### 7.1. Reminder: Add-1 (Laplace) Smoothing

$$P_{Add-1}(w_i \| w_{i-1}) = \frac{c(w_{i-1},w_i) + 1}{c(w_{i-1} + V)}$$

### 7.2. More general formulations: Add-k

$$P_{Add-k}(w_i \| w_{i-1}) = \frac{c(w_{i-1}, w_i) + k}{c(w_{i-1} + kV)}$$

$$P_{Add-k}(w_i \| w_{i-1}) = \frac {c(w_{i-1}, w_i)+m(\frac1V)}{c(w_{i-1})+m}$$

### 7.3. Unigram prior smoothing

$$P_{Add-k}(w_i \| w_{i-1}) = \frac {c(w_{i-1}, w_i) + m(\frac 1V)}{c(w_{i-1}) + m)}$$

$$P_{UnigramPrior}(w_i \| w_{i-1}) = \frac {c(w_{i-1}, w_i) + mP(w_i)}{c(w_{i-1}) + m}$$

### 7.4. Advanced smoothing algorithms

- intuition used by many smoothing algorithms
    - Good-Turing
    - Kneser-Ney
    - Witten-Bell
- use the count of things we've seen once
    - to help estimate the count of things we've never seen

### 7.5. Notation: $$N_c$$ = Frequency of frequency c

$$N_c$$ = the count of things we've seen c times


example: Sam I am I am Sam I do not eat

| Word | Frequnecy |
|:-----|:----------|
| I    | 3         |
| sam  | 2         |
| am   | 2         |
| do   | 1         |
| not  | 1         |
| eat  | 1         |

- $$N_1$$ = 3
- $$N_2$$ = 2
- $$N_3$$ = 1 

### 7.6. Good-Turing smoothing intuition

- You are fishing (a scenario from Josh Goodman), and caught
    - 10 carp, 3 perch, 2 whitefish, 1 trout, 1 salmon, 1 eel = 18 fish
- How likely is it that next species is trout?
    - 1/18
- How likely is it that next species is new (i.e. catfish or bass)
    - let's use our estimate of things-we-saw-once to estimate the new things
    - 3/18 (because N_1 = 3)
- assuming so, how likely is it that next species is trout?
    - must be less than 1/18
    - how to estimate?

### 7.7. Good Turing calculations

$$P_{GT}(things\,with\,zero\,frequency) = \frac{N_1}{N}$$

$$C^{*} = \frac{(c+1)N_{c+1}}{N_c}$$

|            | unseen (bass or catfish)    | seen once (trout)    |
|:-----------|:----------------------------|:---------------------|
| c          | c=0                         | c=1                  |
| MLE        | p=0/18=0                    | p = 1/18             |
| $$P_{GT}$$ | P_GT(unseen) = N_1/N = 3/18 | C(trout) = 2 * N2/N1 = 2 * 1/3 = 2/3 , P_GT(trout) = 2/3 / 18 = 1/27 |



### 7.8. Ney et al.'s Good Turing Intuition

datasets: training, held-out

- intuition from leave-one-out validation
    - take each of the c training words out in turn
    - c training sets of size c-1, held-out of size 1
    - what fraction of held-out words are unseen in training: N_1 / C
    - what fraction of held-out words are seen k times in training: k times, (k+1)N_{k+1}/C
    - so in the future we expect (k+1)N_(k+1)/c of the words to be those with training count k
    - there are N_k words with training count k
    - each should occur with probability (k+1)N_{k+1}/c/N_k
    - or expected count: k* = (k+1)N_{k+1} / N_k
        

### 7.9. Good-Turing complications

- problems: what about "the"? (say c=4417)
    - for small k, N_k > N_{k+1}
    - for large k, too jumpy, zeros wrech estimates
- simple Good-Turing [Gale and Sampson]: replace empirical N_k with a best-fit power law once count counts get unreliable


### 7.10. Resulting Good-Turing numbers

- numbers from Church and Gale (1991)
- 22 million words of AP Newswire

$$c^{*} = \frac{(c+1)N_{c+1}}{N_c}$$


| Count c | Good Turing c* |
|:--------|:---------------|
| 0       | .0000270       |
| 1       | 0.446          |
| 2       | 1.26           |
| 3       | 2.24           |
| 4       | 3.24           |
| 5       | 4.22           |
| 6       | 4.19           |
| 7       | 6.21           |
| 8       | 7.24           |
| 9       | 8.25           | 

## 8. Kneser-Ney Smoothing

### 8.1. Absolute Discounting Interpolation

- save ourselves some time and just subtract 0.75 (or some d)!

$$P_{AbsoluteDiscounting}(w_i \| w_{i-1}) = discounted bigram + \lambda * InterpolationWeight * unigram$$

$$P_{AbsoluteDiscounting}(w_i \| w_{i-1}) = \frac {c(w_{i-1}, w_i) - d}{c(w_{i-1})} + \lambda(w_{i-1})P(w)$$

- maybe keeping a couple extra values of d for counts 1 and 2
- but should we really just use the regular unigram P(w)?

### 8.2. Kneser-Ney Smoothing I

- better estimate for probabilities of lower-order unigrams
    - Shannon game: I can't see without my reading ______ ?
    - "Francisco" is more common than "glasses"
    - but "Francisco" always follows "San"
- the unigram is useful exactly when we haven't seen this bigram!
- instead of P(w): "How likely is w"
- P_continuation(w): "How likely is w to appear as a novel continuation?
    - for each word, count the number of bigram types it completes
    - every bigram type was a novel continuation the first time it was seen

$$P_{CONTINUATION}(w): \|\{w_{i-1}: c(w_{i-1},w) > 0\}\|$$


### 8.3. Kneser-Ney Smoothing II

- how many times does w appear as a novel continuation

$$P_{CONTINUATION}(w): \|\{w_{i-1}: c(w_{i-1},w) > 0\}\|$$

- normalized by the total number of word bigram types

$$\|\{(w_{j-1},w_j):c(w_{j-1},w_j)>0\}\|$$

$$P_{CONTINUATION}(w) = \frac {\|\{w_{i-1}: c(w_{i-1},w) > 0\}\|}{\|\{(w_{j-1},w_j):c(w_{j-1},w_j)\}\|}$$


### 8.4. Kneser-Ney Smoothing III

- alternative metaphor: the number of # of word types seen to precede w

$$\|\{w_{i-1}: c(w_{i-1}, w)>0\}\|$$

- normalized by the # of words preceding all words

$$ P_{CONTINUATION}(w) = \frac {\|\{w_{i-1}: c(w_{i-1}, w)>0\}\|}
                               {\sum_{w'} \|\{w'_{i-1}: c(w'_{i-1}, w')>0\}\|}
$$

- a frequent word (Francisco) occurring in only one context (San) will have a low continuation probability

### 8.5. Kneser-Ney Smoothing IV

$$P_{KN}(w_i \| w_{i-1}) = \frac {max(c(w_{i-1}, w_i)-d,0)}{c(w_{i-1})} +
                           \lambda(w_{i-1}P_{CONTINUATION}(w_i)
$$

$$\lambda$$ is a normalizing constant; the probability mass we've discounted

$$\lambda(w_{i-1}) = theNormalizedDiscount * theNumberOfWordTypesThatCanFollowWi-1$$

the number of word types that can follow w_i-1  
= # of word types we discounted
= # of times we applied normalized discount

$$\lambda(w_{i-1}) = \frac{d}{c(w_{i-1})} \|\{w: c(w_{i-1},w)>0\}\|$$

### 8.6. Kneser-Ney Smoothing: Recursive formulation

$$ P_{KN}(w_i \| w_{i-n+1}^{i-1}) = \frac {max(c_{KN}(w_{i-n+1}^i)-d,0)}{c_{KN}(w_{i-n+1}^{i-1})} +
                                    \lambda(w_{i-n+1}^{i-1}P_{KN}(w_i \| w_{i-n+2}^{i-1}))
$$

$$C_{KN}(\circ) = \{ count(\circ) \qquad for\,the\,highest\,order$$

$$C_{KN}(\circ) = \{ continuationcount(\circ) \qquad for\,lower\,order$$

continuation count = Number of unique single word contexts for $$\circ$$

## References

1. video courses: [part 1][course-1], [part 2][course-2], [part 3][course-3], [part 4][course-4], [part 5][course-5], [part 6][course-6], [part 7][course-7], [part 8][course-8]

  [course-1]: https://www.youtube.com/watch?v=s3kKlUBa3b0&list=PL6397E4B26D00A269&index=12 
  [course-2]: https://www.youtube.com/watch?v=o-CvoOkVrnY&index=13&list=PL6397E4B26D00A269 
  [course-3]: https://www.youtube.com/watch?v=OHyVNCvnsTo&index=14&list=PL6397E4B26D00A269 
  [course-4]: https://www.youtube.com/watch?v=s5Yg6qac9ag&index=15&list=PL6397E4B26D00A269 
  [course-5]: https://www.youtube.com/watch?v=d8nVJjlMOYo&list=PL6397E4B26D00A269&index=16 
  [course-6]: https://www.youtube.com/watch?v=-aMYz1tMfPg&list=PL6397E4B26D00A269&index=17 
  [course-7]: https://www.youtube.com/watch?v=XdjCCkFUBKU&index=18&list=PL6397E4B26D00A269 
  [course-8]: https://www.youtube.com/watch?v=wtB00EczoCM&list=PL6397E4B26D00A269&index=19
