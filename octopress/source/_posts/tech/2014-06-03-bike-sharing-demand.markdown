---
layout: post_tech
title: "Bike Sharing Demand"
date: 2014-06-03 14:49:22 +0800
comments: true
categories: [tech]
tags: [prediction, machine learning, python]
---

The project is to predict the count of bikes sharing per day per hour.

- [description]: bike sharing demand at Kaggle
- [data]: data source of this project at Kaggle
- [codes]: the source codes of this project are available at Github

[description]: http://www.kaggle.com/c/bike-sharing-demand
[data]: http://www.kaggle.com/c/bike-sharing-demand/data
[codes]: https://github.com/KellyChan/kaggle-bike-sharing-demand

## 1. Features

prediction: count = casual + registered

```
count - number of total rentals
casual - number of non-registered user rentals initiated
registered - number of registered user rentals initiated
```

features

```
datetime - hourly date + timestamp  
season -  1 = spring, 2 = summer, 3 = fall, 4 = winter 
holiday - whether the day is considered a holiday
workingday - whether the day is neither a weekend nor holiday
weather - 1: Clear, Few clouds, Partly cloudy, Partly cloudy 
          2: Mist + Cloudy, Mist + Broken clouds, Mist + Few clouds, Mist 
          3: Light Snow, Light Rain + Thunderstorm + Scattered clouds, Light Rain + Scattered clouds 
          4: Heavy Rain + Ice Pallets + Thunderstorm + Mist, Snow + Fog 
temp - temperature in Celsius
atemp - "feels like" temperature in Celsius
humidity - relative humidity
windspeed - wind speed
```

## 2. Evaluation

Submissions are evaluated one the Root Mean Squared Logarithmic Error (RMSLE). The RMSLE is calculated as

$$\sqrt{\frac 1n \sum_1^n (log(p_i + 1) - log(a_i + 1))^2 }$$

Where:

- $$n$$ is the number of hours in the test set
- $$p_i$$ is your predicted count
- $$a_i$$ is the actual count
- $$log(x)$$ is the natural logarithm

## 3. Algorithms

algorithms:

- decision tree regressor
- extra tree regressor
- random forest regressor

## 4. Practice

```python
dataPath = "G:/vimFiles/python/kaggle/201406-bike/data/"
outPath = "G:/vimFiles/python/kaggle/201406-bike/src/outputs/results/"

import pandas as pd

from sklearn.tree import DecisionTreeRegressor
from sklearn.ensemble import ExtraTreesRegressor
from sklearn.ensemble import RandomForestRegressor

def loadData(datafile):
    return pd.read_csv(datafile)

def splitDatetime(data):
    sub = pd.DataFrame(data.datetime.str.split(' ').tolist(), columns = "date time".split())
    date = pd.DataFrame(sub.date.str.split('-').tolist(), columns="year month day".split())
    time = pd.DataFrame(sub.time.str.split(':').tolist(), columns = "hour minute second".split())
    data['year'] = date['year']
    data['month'] = date['month']
    data['day'] = date['day']
    data['hour'] = time['hour'].astype(int)
    return data

def createDecisionTree():
    est = DecisionTreeRegressor()
    return est

def createRandomForest():
    est = RandomForestRegressor(n_estimators=100)
    return est

def createExtraTree():
    est = ExtraTreesRegressor()
    return est

def predict(est, train, test, features, target):

    est.fit(train[features], train[target])

    with open(outPath + "submission-randomforest.csv", 'wb') as f:
        f.write("datetime,count\n")

        for index, value in enumerate(list(est.predict(test[features]))):
            f.write("%s,%s\n" % (test['datetime'].loc[index], int(value)))


def main():

    train = loadData(dataPath + "train.csv")
    test = loadData(dataPath + "test.csv")

    train = splitDatetime(train)
    test = splitDatetime(test)

    target = 'count'
    features = [col for col in train.columns if col not in ['datetime', 'casual', 'registered', 'count']]

    est = createRandomForest()
    predict(est, train, test, features, target)



if __name__ == "__main__":
    main()
```
