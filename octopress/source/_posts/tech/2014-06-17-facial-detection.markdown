---
layout: post_tech
title: "Facial Detection"
date: 2014-06-17 10:04:55 +0800
comments: true
categories: [tech]
tags: [machine learning, computer vision]
---

## 1. Definitions

- Facial Detection: where is the face?
- Facial Recognition: who is this?

## 2. History

| Year | Authors            | Method                       |
|:-----|:-------------------|:-----------------------------|
| 1973 | Kanade             | First automated system       |
| 1987 | Sirovich & Kirby   | Principal Component Analysis |
| 1991 | Turk & Pentland    | Eigenface                    |
| 1996 | Etemad & Chelapa   | Fisherface                   |
| 2001 | Viola & Jones      | AdaBoost + Haar Cascade      |
| 2007 | Naruniec & Skarbek | Gabor Jets                   |

## 3. Concepts

How can a computer find a face?

- skin color (color image)
- motion (video)
- head shape
- all of these combined

Haar Cascade:

- features: edge, line, center-surround
- How is the haar-like feature work?
  - pick a scale, slide it
  - compute the average pixel values under the white area and the black area
  - if the difference between the areas is above some threshold, the feature matches.
- How could this detect a face?
  - the eyes are usually darker than skin

## 4. Classification

- weak classifier
- strong classifier: adaboost - selecting the best weak classifier and then combining the best classifiers

| Data Point | Classifier 1 | Classifier 2 | Classifier 3 |
|:-----------|:-------------|:-------------|:-------------|
| x1         | pass         | fail         | pass         |
| x2         | fail         | pass         | pass         |
| x3         | pass         | pass         | pass         |
| x4         | fail         | fail         | pass         |


## References

- Facial Detection: [Part 1](https://www.youtube.com/watch?v=sWTvK72-SPU)
- [OpenCV](http://docs.opencv.org/trunk/doc/py_tutorials/py_setup/py_setup_in_windows/py_setup_in_windows.html)
