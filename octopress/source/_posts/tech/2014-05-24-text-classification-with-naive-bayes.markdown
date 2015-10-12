---
layout: post_tech
title: "Text Classification with Naive Bayes"
date: 2014-05-24 01:05:22 +0800
comments: true
categories: [tech]
tags: [nlp, machine learning, classification, python, nltk]
---

## 1. Working Process

general process

```
raw text --> keywords --> features --> train --> clasifier
raw text              --> features --> test  --> classify
```

example: sentimental analysis for tweets

```
positive/negative tweets --> keywords --> features --> train --> clasifier
unlabeled tweets                      --> features --> test  --> classify
```

## 2. Feature Extraction

keyword extraction: wordFreqDict

```
wordFreqDict = {keyword1: 90,
                keyword2: 30,
                keyword3: 3,
                ...}
```

feature extraction: True/False table


| tweet                | keyword1 | keyword2 | keyword3 | ... |
|:---------------------|:---------|:---------|:---------|:----|
| "I like this dog."   | True     | False    | True     | ... |
| "Beautiful morning." | False    | False    | False    | ... |
| ...                  | ...      | ...      | ...      | ... |

.

train data: features + label

| tweet                | keyword1 | keyword2 | keyword3 | ... | label    |
|:---------------------|:---------|:---------|:---------|:----|:---------|
| "I like this dog."   | True     | False    | True     | ... | Positive |
| "Beautiful morning." | False    | False    | False    | ... | Positive |
| ...                  | ...      | ...      | ...      | ... | ...      |

.

## 3. Practice

function construct

```
[predClass]: classifyNB(testX, train, keywords)
    |----- [test-features]: extractFeatures(dataX, keywords)
    |----- [train]: createTrain(fullData, keywords)
              |----- [features]: extractFeatures(dataX, keywords)
              |----- [keywords]: createKeywords(fullData)
                        |----- [fullData]: generateFullData(data1, data2)
                                     |----- [rawData]: loadData()
```

data format

```
- rawData: [('word word ...', 'label'), ... ]
- fullData: [([words], label), ([words], label), ...]

- features: {word: True/False}
- train: [({features}, label), ({features}, label), ...]
- test: ['word word ...', 'word word ...', ...]
```

source

```python
import nltk

def loadData():
    """ loading raw data """
    
    pos_tweets = [('I love this car', 'positive'),
                  ('This view is amazing', 'positive'),
                  ('I feel great this morning', 'positive'),
                  ('I am so excited about the concert', 'positive'),
                  ('He is my best friend', 'positive')]
    
    neg_tweets = [('I do not like this car', 'negative'),
                  ('This view is horrible', 'negative'),
                  ('I feel tired this morning', 'negative'),
                  ('I am not looking forward to the concert', 'negative'),
                  ('He is my enemy', 'negative')]
    
    return pos_tweets, neg_tweets

def generateFullData(data1, data2):
    """ generating full data with feature and label 
    
    fullData: [([words], label), ([words], label), ...]
    """
    
    fullData = []
    
    for (words, label) in data1 + data2:
        words_filterred = [e.lower() for e in words.split() if len(e) >= 3]
        fullData.append((words_filterred, label))
    return fullData

def createKeywords(data):
    """ generating word dictionary from all words """
    
    wordlist = []
    for (words, label) in data:
        wordlist.extend(words)  # combining all words in a list
    
    wordDict = nltk.FreqDist(wordlist)  # generating frquencies of each word
    return wordDict.keys() 

def extractFeatures(dataX, keywords):
    """ extracting word table with true/false 
    
    features: {word: True/False}
    """
    
    dataX_wordSet = set(dataX)
    
    features = {}
    for word in keywords:
        # creating true/false table
        features['contains(%s)' % word] = (word in dataX_wordSet)  
    return features

def createTrain(data, keywords):
    """ creating train data
    
    train: [({features}, label), ({features}, label), ...]
    - features: {word: True/False}
    """
    
    train = []
    for record in data:
        # format: ({word: True/False}, label)
        train.append((extractFeatures(record[0], keywords), record[1]))  

    # if only have one argument, it also can be written by apply_features
    #train = nltk.classify.apply_features(extractFeatures, data)
    
    return train

def classifyNB(testX, train, keywords):
    """ classifying label with naive bayes """
    
    # creating Naive Bayes classifier
    classifier = nltk.NaiveBayesClassifier.train(train)
    
    # extracting features from raw testX and predicting label
    testX = extractFeatures(testX.split(), keywords)
    predClass = classifier.classify(testX)
    return predClass
    

def main():
    
    # loading data and combining as one
    pos_tweets, neg_tweets = loadData()
    tweets = generateFullData(pos_tweets, neg_tweets)
    
    # extracting keywords and train data
    keywords = createKeywords(tweets)
    train = createTrain(tweets, keywords)
    
    # predicting label
    testX = 'Larry is my friend'
    predClass = classifyNB(testX, train, keywords)
    print predClass
    

if __name__ == '__main__':
    main()
```

## Reference

1. [Twitter sentiment analysis using Python and NLTK][twitter-sentiment-analysis-using-python-and-nltk]

   [twitter-sentiment-analysis-using-python-and-nltk]: http://www.laurentluce.com/posts/twitter-sentiment-analysis-using-python-and-nltk/
