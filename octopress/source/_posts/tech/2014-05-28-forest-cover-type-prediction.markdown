---
layout: post_tech
title: "Forest Cover Type Prediction"
date: 2014-05-28 15:00:00 +0800
comments: true
categories: [tech]
tags: [classification, machine learning, python]
---

This project is to predict the forest cover type with the cartography data provided by US Geological Survey and USFS.

- [description]: forest cover type prediction at Kaggle
- [data]: data source of this project at Kaggle
- [codes]: the source codes of this project are available at Github


[description]: http://www.kaggle.com/c/forest-cover-type-prediction
[data]: http://www.kaggle.com/c/forest-cover-type-prediction/data
[codes]: https://github.com/KellyChan/kaggle-forest-cover-type-prediction

## 1. Working Process

- step 1. combining binary variables (Wilderness_Area and Soil_Type) as one
- step 2. applying machine learning algorithms (random forest)
- step 3. evaluating the accuracy of prediction

## 2. Preprocessing

combining binary variables (Wilderness_Area, Soil_Type)

## 3. Classification

datasets

- raw data
- punched data

algorithms

- decision tree
- random forest
- extra tree: raw data worked much better than punched data.
- adaboost

## 4. Evaluation

% of corrections

## 5. Practice

### step1. data punching 

combining binary variables - Wilderness_Area, Soil_Type

NOTE: Pandas is good at handling with data by batch, it is much faster than processing each record one by one.


```python
dataPath = "path/kaggle/201405-Forest/data/"
outPath = "path/kaggle/201405-Forest/src/outputs/data/"

import pandas as pd

def loadData(datafile):
    return pd.read_csv(datafile)

def createWildernessArea(data):

    for i in range(4):
        coded = {'1': i+1, '0': 0}
        col = 'Wilderness_Area' + str(i+1)
        data[col] = data[col].astype(str).map(coded)

    data['Wilderness_Area'] = data['Wilderness_Area1'] + \
                              data['Wilderness_Area2'] + \
                              data['Wilderness_Area3'] + \
                              data['Wilderness_Area4']
 
    for i in range(4):
        col = 'Wilderness_Area' + str(i+1)
        data = data.drop(col, 1)

    return data

def createSoilType(data):

    for i in range(40):
        coded = {'1': i+1, '0': 0}
        col = 'Soil_Type' + str(i+1)
        data[col] = data[col].astype(str).map(coded)    

    data['Soil_Type'] = 0
    for i in range(40):
        col = 'Soil_Type' + str(i+1)
        data['Soil_Type'] += data[col] 
 
    for i in range(40):
        col = 'Soil_Type' + str(i+1)
        data = data.drop(col, 1)

    return data

def main():
    train = loadData(dataPath + "train.csv")
    test = loadData(dataPath + "test.csv")
    
    train = createWildernessArea(train)
    train = createSoilType(train)
    train.to_csv(outPath + "train.csv")

    test = createWildernessArea(test)
    test = createSoilType(test)
    test.to_csv(outPath + "test.csv")


if __name__ == '__main__':
    main()
```

### step2. classification with trees

```python
"""
Function Tree

- chooseDataset
     |------ loadData

- chooseAlgorithms
     |------ createDecisionTree
     |------ createRandomForest
     |------ createExtraTree
     |------ createAdaBoost

"""

rawPath = "G:/vimFiles/python/kaggle/201405-Forest/data/"
dataPath = "G:/vimFiles/python/kaggle/201405-Forest/src/outputs/data/"
outPath = "G:/vimFiles/python/kaggle/201405-Forest/src/outputs/results/"

import pandas as pd

from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.ensemble import ExtraTreesClassifier
from sklearn.ensemble import AdaBoostClassifier

def loadData(datafile):
    return pd.read_csv(datafile)


def createDecisionTree():
    clf = DecisionTreeClassifier(max_depth=None, min_samples_split=1, random_state=0)
    return clf

def createRandomForest():
    clf = RandomForestClassifier(n_estimators=500, max_depth=None, min_samples_split=1, random_state=0)
    return clf

def createExtraTree():
    clf = ExtraTreesClassifier(n_estimators=500, max_depth=None, min_samples_split=1, random_state=0)
    return clf

def createAdaBoost():
    dt = DecisionTreeClassifier(max_depth=None, min_samples_split=1, random_state=0)
    clf = AdaBoostClassifier(dt, n_estimators=300)
    return clf

def classify(clf, train, cols, target, test, filePath):
    clf.fit(train[cols], train[target])

    with open(filePath, "wb") as outfile:
        outfile.write("Id,Cover_Type\n")
        for index, value in enumerate(list(clf.predict(test[cols]))):
            outfile.write("%s,%s\n" % (test['Id'].loc[index], value))


def chooseDataset(dataset):
    if dataset == 1:
        train = loadData(rawPath + "train.csv")
        test = loadData(rawPath + "test.csv")
    elif dataset == 2:
        train = loadData(dataPath + "train.csv")
        test = loadData(dataPath + "test.csv")
    return train, test

def chooseAlgorithms(algo, train, test, target, features):

    if algo == 0:

        clf = createDecisionTree()
        classify(clf, train, features, target, test, outPath+"submission-decisiontree.csv")        

        clf = createRandomForest()
        classify(clf, train, features, target, test, outPath+"submission-randomforest.csv")

        clf = createExtraTree()
        classify(clf, train, features, target, test, outPath+"submission-extratree.csv")

        clf = createAdaBoost()
        classify(clf, train, features, target, test, outPath+"submission-adaboost.csv")

    elif algo == 1:
        clf = createDecisionTree()
        classify(clf, train, features, target, test, outPath+"submission-decisiontree.csv")

    elif algo == 2:
        clf = createRandomForest()
        classify(clf, train, features, target, test, outPath+"submission-randomforest.csv")
    
    elif algo == 3:
        clf = createExtraTree()
        classify(clf, train, features, target, test, outPath+"submission-extratree.csv")

    elif algo == 4:
        clf = createAdaBoost()
        classify(clf, train, features, target, test, outPath+"submission-adaboost.csv")


def main():

    """ selections: dataset and algorithms

    dataset:
    - 1: raw data - Wilderness_Area, Soil_Type are binary codes
    - 2: punched data - Wilderness_Area, Soil_Type are combined

    algo: algorithms
    - 0: all
    - 1: decision tree
    - 2: random forest
    - 3: extra tree
    - 4: adaboost
    """
    dataset = 1
    algo = 1

    train, test = chooseDataset(dataset)

    target = 'Cover_Type'
    features = [col for col in train.columns if col not in ['Id', 'Cover_Type', 'Unnamed']]

    chooseAlgorithms(algo, train, test, target, features)



if __name__ == '__main__':
    main()
```

## 6. Data formats

predict(submission): `Id`, `Cover_Type`


```
Cover_Type (7 types, integers 1 to 7) - Forest Cover Type designation
         
1 - Spruce/Fir
2 - Lodgepole Pine
3 - Ponderosa Pine
4 - Cottonwood/Willow
5 - Aspen
6 - Douglas-fir
7 - Krummholz
```

train/test: `Id`

features

```
Elevation - Elevation in meters
Aspect - Aspect in degrees azimuth
Slope - Slope in degrees

Horizontal_Distance_To_Hydrology - Horz Dist to nearest surface water features
Vertical_Distance_To_Hydrology - Vert Dist to nearest surface water features
Horizontal_Distance_To_Roadways - Horz Dist to nearest roadway
Horizontal_Distance_To_Fire_Points - Horz Dist to nearest wildfire ignition points

Hillshade_9am (0 to 255 index) - Hillshade index at 9am, summer solstice
Hillshade_Noon (0 to 255 index) - Hillshade index at noon, summer solstice
Hillshade_3pm (0 to 255 index) - Hillshade index at 3pm, summer solstice

Wilderness_Area (4 areas, 0 = absence, 1 = presence) - Wilderness area designation
- 1 - Rawah Wilderness Area
- 2 - Neota Wilderness Area
- 3 - Comanche Peak Wilderness Area
- 4 - Cache la Poudre Wilderness Area

Soil_Type (40 types, 0 = absence, 1 = presence) - Soil Type designation
- 1 Cathedral family - Rock outcrop complex, extremely stony. 
- 2 Vanet - Ratake families complex, very stony.
- 3 Haploborolis - Rock outcrop complex, rubbly.
- 4 Ratake family - Rock outcrop complex, rubbly.
- 5 Vanet family - Rock outcrop complex complex, rubbly.
- 6 Vanet - Wetmore families - Rock outcrop complex, stony.
- 7 Gothic family.
- 8 Supervisor - Limber families complex.
- 9 Troutville family, very stony.
- 10 Bullwark - Catamount families - Rock outcrop complex, rubbly.
- 11 Bullwark - Catamount families - Rock land complex, rubbly.
- 12 Legault family - Rock land complex, stony.
- 13 Catamount family - Rock land - Bullwark family complex, rubbly.
- 14 Pachic Argiborolis - Aquolis complex.
- 15 unspecified in the USFS Soil and ELU Survey.
- 16 Cryaquolis - Cryoborolis complex.
- 17 Gateview family - Cryaquolis complex.
- 18 Rogert family, very stony.
- 19 Typic Cryaquolis - Borohemists complex.
- 20 Typic Cryaquepts - Typic Cryaquolls complex.
- 21 Typic Cryaquolls - Leighcan family, till substratum complex.
- 22 Leighcan family, till substratum, extremely bouldery.
- 23 Leighcan family, till substratum - Typic Cryaquolls complex.
- 24 Leighcan family, extremely stony.
- 25 Leighcan family, warm, extremely stony.
- 26 Granile - Catamount families complex, very stony.
- 27 Leighcan family, warm - Rock outcrop complex, extremely stony.
- 28 Leighcan family - Rock outcrop complex, extremely stony.
- 29 Como - Legault families complex, extremely stony.
- 30 Como family - Rock land - Legault family complex, extremely stony.
- 31 Leighcan - Catamount families complex, extremely stony.
- 32 Catamount family - Rock outcrop - Leighcan family complex, extremely stony.
- 33 Leighcan - Catamount families - Rock outcrop complex, extremely stony.
- 34 Cryorthents - Rock land complex, extremely stony.
- 35 Cryumbrepts - Rock outcrop - Cryaquepts complex.
- 36 Bross family - Rock land - Cryumbrepts complex, extremely stony.
- 37 Rock outcrop - Cryumbrepts - Cryorthents complex, extremely stony.
- 38 Leighcan - Moran families - Cryaquolls complex, extremely stony.
- 39 Moran family - Cryorthents - Leighcan family complex, extremely stony.
- 40 Moran family - Cryorthents - Rock land complex, extremely stony.
```

## Reference

- [Aspect Geography](http://en.wikipedia.org/wiki/Aspect_(geography))
