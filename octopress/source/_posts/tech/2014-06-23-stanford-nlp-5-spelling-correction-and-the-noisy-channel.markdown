---
layout: post_tech
title:  "Stanford NLP 5 - Spelling Correction and the Noisy Channel"
date:   2014-06-23 20:25:12 +0800
comments: true
categories: [tech]
tags: [nlp, machine learning]
---

## 1. The Spelling Correction Task

### 1.1. Applications for spelling correction

- word processing
- search engine
- phones

### 1.2. Spelling Tasks

- spelling error detection
- spelling error correction
    - autocorrect: hte -> the
    - suggest a correction
    - suggestion lists

### 1.3. Types of spelling errors

- non-word errors
    - graffe -> giraffe
- real-word errors
    - typographical erros: three -> there
    - cognitive errors (homophones): piece -> peace, too -> two

### 1.4. Rates of spelling errors

|      |                                                   |                                            |
|:-----|:--------------------------------------------------|:-------------------------------------------|
|  26% | web queries                                       | Wang et al. 2003                           |
|  13% | retyping, no backspace                            | Whitelaw et al. English & German           |
|   7% | words corrected retyping on phone-sized organizer |                                            |
|   2% | words uncorrected on organizer                    | Soukoreff & MacKenzie 2003                 |
| 1-2% | retyping                                          | Kane and Wobbrock 2007, Gruden et al. 1983 |

### 1.5. Non-word spelling errors

- non-word spelling error detection
    - any word not in a dictionary is an error
    - the larger the dictionary the better
- non-word spelling error correction
    - generate candidates: real words that are similar to error
    - choose the one which is best
        - shorest weighted edit distance
        - highest noisy channel probability

### 1.6. Real word spelling errors

- for each word w, generate candidate set:
    - find candidate words with similar pronunciations
    - find candidate words with similar spelling
    - include w in candidate set
- choose beset candidate
    - noisy channel
    - classifier


## 2. The Noisy Channel Model of Spelling

### 2.1. Noisy Channel Intuition

original word -- (noisy channel) --> noisy word -- (decoder) --> guessed word

### 2.2. Noisy Channel

- we see an observation x of a misspelled word
- find the correct word w: bayes rule

channel/error model: maximize the product of two factors - likelihood and prior <- language model

$$
\hat w = argmax_{w \in V} P(w \| x) 
       = argmax_{x \in V} \frac {P(x \| w)P(w)}{P(x)}
       = argmax_{x \in V} P(x \| w)P(w)
       = argmax_{likelihood * prior}
$$


### 2.3. History: Noisy channel for spelling proposed around 1990

- IBM
    - Mays, Eric, Fred J. Damerau and Robert L. Mercer. 1991
    - Context based spelling correction. Inforamtion Processing and management, 23(5), 517-522
- AT&T Bell Labs
    - Kernighan, Mark D., Kenneth W. Church, and William A. Gale. 1990
    - A spelling correction program based on a nosiy channel model. Proceedings of COLING 1990, 205-210

### 2.4. Non-word spelling error example

acress

### 2.5. Candidate generation

- words with similar spelling
    - small edit distance to error 
- words with similar pronunciation
    - small edit distance of pronunciation to error

### 2.6. Damerau-Levenshtein edit distance

- minimal edit distance between two strings, where edits are
    - insertion
    - deletion
    - substitution
    - transposition of two adjacent letters

### 2.7. Words within 1 of acress

| Error | Candidate Correction | Correct Letter | Error Letter | Type          |
|:------|:---------------------|:---------------|:-------------|:--------------|
| acress | actress             | t              | -            | deletion      |
| acress | cress               | -              | a            | insertion     |
| acress | caress              | ca             | ac           | transposition |
| acress | access              | c              | r            | substitution  |
| acress | across              | o              | e            | substitution  |
| acress | across              | o              | e            | substitution  |
| acress | acres               | -              | s            | insertion     |
| acress | acres               | -              | s            | insertion     |


### 2.8. Candidate generation

- 80% of erros are within edit distance 1
- almost all errors within edit distance 2

practice:

- also allow insertion of space or hyphen
    - thisidea -> this idea
    - inlaw -> in-law

### 2.9. Language Model

- use any of the language modeling algorithms we've learned
- unigram, bigram, trigram
- web-scale spelling correction
    - stupid backoff

### 2.10. Unigram Prior probability

counts from 404,253,213 words in Corpus of Contemporary Englsih (COCA)

| word    | Frequency of word | P(word)     |
|:--------|:------------------|:------------|
| actress | 9,321             | .0000230573 |
| cress   | 220               | .0000005442 |
| caress  | 686               | .0000016969 |
| access  | 37,038            | .0000916207 |
| across  | 120,844           | .0002989314 | 
| acres   | 12,874            | .0000318463 |

## 3. Real-Word Spelling Correction

## 4. State of the Art Systems


## References

1. video courses: [part 1][course-1], [part 2][course-2], [part 3][course-3], [part 4][course-4]

  [course-1]: https://www.youtube.com/watch?v=Z1m7McLIP9c&index=20&list=PL6397E4B26D00A269
  [course-2]: https://www.youtube.com/watch?v=RgHr2KVXtiE&index=21&list=PL6397E4B26D00A269
  [course-3]:
  [course-4]: 
