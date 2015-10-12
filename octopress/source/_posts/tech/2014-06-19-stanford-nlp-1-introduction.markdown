---
layout: post_tech
title: "Stanford NLP 1 - Introduction"
date: 2014-06-19 17:42:24 +0800
comments: true
categories: [tech]
tags: [nlp, machine learning]
---

## 1. What is Natural Language Processing?

Answers

- IBM’s Watson (Feb 16 2011)
- Information Extraction
  - email: creating a new calendar entry from email
  - sentiment analysis: product feedback
- Machine Translation
  - fully automatic
  - helping human translators 

language Technology

| mostly solved                  | making good progress            | still really hard       |
|:-------------------------------|:--------------------------------|:------------------------|
| Spam detection                 | Sentiment analysis              | Question answering (QA) |
| Part-of-speech (POS) tagging   | Coreference resolution          | Paraphrase              |
| Named entity recognition (NER) | Word sense disambiguation (WSD) | Summarization           |
|                                | Parsing                         | Dialog                  |
|                                | Machine translation (MT)        |                         |
|                                | Information extraction (IE)     |                         |


## 2. The Hard Problems

### 2.1. Ambiguity makes NLP hard: “Crash blossoms”

Type 1:

Violinist Linked to JAL Crash Blossoms

- [Violinist] Linked to [JAL Crash Blossoms]
- [Violinist Linked to JAL] Crash [Blossoms]

Teacher Strikes Idle Kids

- [Teacher Strikes] Idle [Kids]
- [Teacher] Strikes [Idle Kids]


Type 2:

Red Tape Holds Up New Bridges

- Holds Up: delay
- Holds Up: to support


Others:

- Hospitals Are Sued by 7 Foot Doctors
- Juvenile Court to Try Shooting Defendant
- Local High School Dropouts Cut in Half


### 2.2. Ambiguity is pervasive

Fed raises interest rates

phrase structure 1

| Fed | raises  | interest | rates  |
|:---:|:-------:|:--------:|:------:|
| N   | V       | N        | N      |
| NP  |         | NP                |
|     | VP                          |
| S                                 |

phrase structure 2

| Fed | raises  | interest | rates  |
|:---:|:-------:|:--------:|:------:|
| N   | N       |          | N      |
|     |         | V        | VP     |
| NP  |         | VP       |        |
| S                                 |


Fed raises interest rates 0.5%

- [Fed raises interest] rates [0.5%]


## 3. Solutions

Making progress on this problem

The task is difficult! What tools do we need?

- knowledge about language
- knowledge about the world
- a way to combine knowledge sources

How we generally do this:

- probabilistic models built from language data
  - P(“maison” -> “house”) high
  - P(“L’avocat general” -> “the general avocado”) low
- luckily, rough text features can often do half the job


## 4. Class

Teaches key theory and methods for statistical NLP:

- viterbi
- naive bayes, maxent classifiers
- n-gram language modeling
- statistical parsing
- inverted index, tf-idf, vector models of meaning

For practical, robust real-world applications

- information extraction
- spelling correction
- information retrieval
- sentiment analysis

## 5. Skills

- simple linear algebra (vectors, matrics)
- basic probability theory
- java or python programming


## Reference

- [Stanford NLP 1](https://www.youtube.com/watch?v=nfoudtpBV68&index=1&list=PL6397E4B26D00A269)
