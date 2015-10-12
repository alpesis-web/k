---
layout: post_tech
title: "Stanford NLP 2 - Basic Text Processsing"
date: 2014-06-19 20:18:34 +0800
comments: true
categories: [tech]
tags: [nlp, machine learning]
---

## 1. Regular Expressions

A formal language for specifying text strings

## 1.1. Regular Expressions: Disjunctions

How can we search for any of these?

- (singular) woodchuck
- (plural) woodchucks
- (capital) Woodchuck
- (combination) Woodchucks

Regular Expressions:

- letters inside square brackets []
- ranges [A-Z]
- negations [^Ss]
  - carat means negation only when first in []

## 1.2. Regular Expressions: More Disjunction

- Woodchucks is another name for groundhog!
- The pipe | for disjunction

## 1.3. Regular Expressions: ? * + .

## 1.4. Regular Expressions: anchors ^ $

example: find me all instances of the word “the” in a text

- the: [^A-Za-z][Tt]he[^A-Za-z]

## 1.5. Summary

| Pattern                      | Matches                    | Example                 |
|:-----------------------------|:---------------------------|-------------------------|
| [wW]oodchuck                 | Woodchuck, woodchuck       |                         |
| [1234567890]                 | Any digit                  |                         |
| [A-Z]                        | an upper case letter       |                         |
| [a-z]                        | a lower case letter        |                         |
| [0-9]                        | a single digit             |                         |
| [^A-Z]                       | not an upper case letter   |                         |
| [^Ss]                        | neither 'S' nor 's'        |                         |
| [^e^]                        | neither e nor ^            |                         |
| a^b                          | the pattern a carat b      |                         |
| groundhog \| woodchuck       |                            |                         |
| yours \| mine                | yours mine                 |                         |
| a\|b\|c                      | =[abc]                     |                         |
| [gG]roundhog \| [wW]oodchuck |                            |                         |
| oo\*h!                       | 0 or more of previous char | oh! ooh! oooh! ooooh!   |
| o+h!                         | 1 or more of previous char | oh! ooh! oooh! oooh!    |
| baa+                         |                            | baa baaa baaa baaa      |
| beg.n                        | anyone                     | begin begun begun beg3n |
| colou?r                      | optional previous char     | color colour            |
| ^[A-Z]                       | begin of the line          | Palo Alto               |
| ^[A-Za-z]                    | begin of the line          | 1 "Hello"               |
| .$                           | end of the line            | The end.                |
| .$                           | end of the line            | The end? The end!       |

.

**Errors**

The process we just went through was based on fixing two kinds of errors

- (Type I - False positives) matching strings that we should not have matched (there, then, other)
- (Type II - False negatives) not matching things that we should have matched (The)


**Errors Cont.**

in NLP we are always dealing with these kinds of errors

reducing the error rate for an application often involved two antagonistic efforts

- increasing accuracy or precision (minimizing false positives)
- increasing coverage or recall (minimizing false negatives)

**Summary**

regular expressions play a surprisingly large role

- sophisticated sequences of regular expressions are often the first model for any text processing text

for many hard tasks, we use machine learning classifiers

- but regular expressions are used as features in the classifiers
- can be very useful in capturing generalizations



## 2. Word Tokenization

### 2.1. Text Normalization

every NLP task needs to do text

normalization

- segmenting/ tokenizing words in running text
- normalizing word formats
- segmenting sentences in running text


### 2.2. How many words?

I do uh main- mianly business data processing

- fragments, filled pauses


Seuss’s cat in the hat is different from other cats!

- lemma: same stem, part of speech, rough word sense
  - cat and cats = same lemma
- wordform: the full inflected surface form
  - cat and cats = different wordforms


Definitions:

- type: an element of the vocabulary - V = vocabulary = set of types - |V| is the size of the vocabulary
- token: an instance of that type in running text - N = number of token
- Church and Gale (1990): |V| > O(N^1/2)

|                                 | Tokens = N  | Types = \|V\| |
|:--------------------------------|:------------|:--------------|
| Switchboard phone sonversations | 2.4 million | 20 thousand   |
| Shakespeare                     | 884,000     | 31 thousand   |
| Google N-grams                  | 1 trillion  | 13 million    |

example:

They lay back on the San Francisco grass and looked at the starts and their

how many?

- 15 tokens (or 14) -> San Francisco is one word or two words?
- 13 types (or 12)(or 11?)


### 2.3. Issues in Tokenization

|                     |                                  |
|:--------------------|:---------------------------------|
| Finland's capital   | Finland Finlands Finland's?      |
| what're, I'm, isn't | What are, I am, is not           |
| Hewlett-Packard     | Hewlett Packard?                 |
| state-of-the-art    | state of the art?                |
| Lowercase           | lower-case lowercase lower case? |
| m.p.h., PhD.        | ??                               |

### 2.4. Tokenization: language issues

French

- L’ensemble -> one token or two?
  - L? l’? Le?
  - Want l’ensemble to match with un ensemble

German noun compounds are not segmented

- Lebensversicherungsgesellschaftsangestellter
- ‘life insurance company employee’
- German information retrieval needs compound splitter

Chinese and Japanese no spaces between words:

- 莎拉波娃现在居住在美国东南部的佛罗里达
- 莎拉波娃 现在 居住 在 美国 东南部 的 佛罗里达
- Sharapova now lives in US southeastern Florida

Further complicated in Japanese, with multiple alphabets intermingled

- Dates/amounts in multiple formats
- Katakana Hiragana Kanji Romaji

End-user can express query entirely in hiragana!


### 2.5. Word Tokenization in Chinese

also called Word Segmentation

- Chinese words are composed of characters
- characters are generally 1 syllable and 1 morepheme

average word is 2.4 characters long

standard baseline segmentation algorithm:

- maximum matching (also called greedy)


### 2.6. Maximum Matching Word Segmentation Algorithm

given a wordlist of Chinese, and a string

1. start a pointer at the beginning of the string
2. find the longest word in dictionary that matches the string starting at pointer
3. move the pointer over the word in string
4. go to 2


### 2.7. Max-match segmentation illustration

|                   |                      |
|:------------------|:---------------------|
| Thecatinthehat    | the cat in the hat   |
| Thetabledownthere | the table down there |
|                   | theta bled own there |


Doesn’t generally work in English!

But works astonishingly well in Chinese

- 莎拉波娃现在居住在美国东南部的佛罗里达
- 莎拉波娃 现在 居住 在 美国 东南部 的 佛罗里达

Modern probabilistic segmentation algorithms even better


## 3. Word Normalization and Stemming

### 3.1. Normalization

need to “normalize” terms

- information retrieval: indexed text and query terms must have same form
  - we want to match U.S.A and USA

we impicitly define equivalence classes of terms

- e.g., deleting periods in a term

alternative: asymmetric expansion

potentially more powerful, but less efficient


| Enter   | Search                   |
|:--------|:-------------------------|
| window  | window, windows          |
| windows | Windows, windows, window |
| Windows | Windows                  |


### 3.2. Case folding

applications like IR: educe all letters to lower case

- since users tend to use lower case
- possible exception: upper case in mid-sentence?
  - e.g., General Motors
  - Fed vs. fed
  - SAIL vs. sail

for sentiment analysis, MT, Information extraction

- case is helpful (US versus us is important)


### 3.3. Lemmatization

- reduce inflections or variant forms to base form
- lemmatization: have to find correct dictionary headword form
- machine translation
  - Spanish quiero (“I want”), quieres (“you want”) same lemma as querer “want”

| inflections or variant forms  | base form |
|:------------------------------|:----------|
| am, are, is                   | be        |
| car, cars, car's, cars'       | car       |

example:

the boy's cars are different colors -> the boy car be different color


### 3.4. Morphology

morphemes

- the small meaningful units that make up words
- stems: the core meaning-bearing units
- affixes: bits and pieces that adhere to stems
  - often with grammatical functions


### 3.5. Stemming

- reduce terms to their stems in information retrieval
- stemming is crude chopping of affixes
  - language dependent
  - e.g., automate(s), automatic, automation all reduced to automat

example:

for example compressed and compression are both accepted as equivalent to compress

- for exampl compress and compress ar both accept as equival to compress


### 3.6. Porter’s algorithm - the most common English stemmer

step 1a

|      |    |                    |
|:-----|:---|:-------------------|
| sses | s  | caresses -> caress |
| ies  | i  | ponies -> poni     |
| ss   | ss | caress -> caress   |
| s    |  / | cats -> cat        |

step 1b

|            |    |                      |
|:-----------|:---|:---------------------|
| (\*v\*)ing |  / | walking -> walk      |
|            |    | sing -> sing         |
| (\*v\*)ed  |  / | plastered -> plaster |

step 2 (for long stems)

|          |     |                       |
|:---------|:----|:----------------------|
| ational  | ate | relational -> relate  |
| izer     | ize | digitizer -> digitize |
| ator     | ate | operator -> operate   |

step 3 (for longer stems)

|      |     |                      |
|:-----|:----|:---------------------|
| al   |  /  | revival -> reviv     |
| able |  /  | adjustable -> adjust |
| ate  |  /  | activate -> activ    |


### 3.7. Viewing morphology in a corpus - Why only strip -ing if there is a vowel?

|            |    |                 |                           |
|:-----------|:---|:----------------|:--------------------------|
| (\*v\*)ing |  / | walking -> walk | with an earlier vowel "a" |
|            |    | sing -> sing    | without vowel             |

### 3.8. Dealing with complex morphology is sometimes necessary

- some languages requires complex morpheme segmentation
     - Turkish
     - Uygarlastiranoadiklarimizdanmissinizcasina
     - `(behaving) as if you are among those whom we could not civilize`
     - Uygar `civilized` + las `become`
         - `+` tir `cause` + ama `not able`
         - `+` dik `past` + lar `plural`
         - `+` imiz `p1pl` + dan `abl`
         - `+` mis `past` + siniz `2pl` + casina `as if`


## 4. Sentence Segmentation and Decision Trees

### 4.1. Sentence Segmentation

!, ? are relatively unambiguous

Period “.” is quite ambiguous

- Sentence boundary
- Abbreviations like Inc. or Dr.
- Numbers like .02% or 4.3


solution: build a binary classifier

- looks at a “.”
- decides EndOfSentence/ NotEndOfSentence
- classifiers: hand0written rules, regular expressions, or machine learning


### 4.2. Determining if a word is end-of-sentence: a Decision Tree

```
lots of blank lines after me?
    |--- YES ---> E-O-S
    |--- NO  ---> final punctuation is ?, !, or :?
                     |--- YES ---> E-O-S
                     |--- NO  ---> final punctuation is period
                                        |--- YES ---> I am "etc" or other abbreviation
                                                        |--- YES ---> not E-O-S
                                                        |--- NO  ---> E-O-S
                                        |--- NO  ---> not E-O-S
```

### 4.3. More sophisticated decision tree features

case of word with/after “.”: Upper, Lower, Cap, Number

numeric features

- length of word with “.”
- probability(word with “.” occurs at end-of-s)
- probability(word after “.” occurs at beginning-of-s)


### 4.4. Implementing Decision Trees

a decision tree is just an if-then-else statement

the interesting research is choosing the features

setting up the structure is often too hard to do by hand

- hand-building only possible for very simple features, domains
  - for numeric features, it’s too hard to pick each threshold
- instead, structure usually learned by machine learning from a training corpus


### 4.5. Decision Trees and other classifiers

we can think of the questions in a decision tree

as features that could be exploited by an kind of classifier

- logistic regression
- svm
- neural nets
- etc.


## References

1. video courses: [part 1][course-1], [part 2][course-2], [part 3][course-3], [part 4][course-4], [part 5][course-5]
2. tools: [RegexPal][regexpal]

   [course-1]: https://www.youtube.com/watch?v=hwDhO1GLb_4&list=PL6397E4B26D00A269&index=2
   [course-2]: https://www.youtube.com/watch?v=RGLldper5II&index=3&list=PL6397E4B26D00A269
   [course-3]: https://www.youtube.com/watch?v=jBk24DI8kg0&index=4&list=PL6397E4B26D00A269
   [course-4]: https://www.youtube.com/watch?v=2s7f8mBwnko&list=PL6397E4B26D00A269&index=5
   [course-5]: https://www.youtube.com/watch?v=di0N3kXfGYg&index=6&list=PL6397E4B26D00A269

   [regexpal]: regexpal.com
