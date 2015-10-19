---
layout: post_tech
title: "edX Service Ports"
date: 2014-11-15 15:50:09 +0800
comments: true
categories: [tech]
tags: [edX, edX Platform, edX Analytics]
toc: false
---

## 1. edX Platform

| server       | dev   | production |
|:-------------|:------|:-----------|
| mongod       | 27017 |            |
| mysql        | 3306  |            |
| Rabbit MQ    | 5672  | 5672       |
| lms          | 8000  |            |
| cms          | 8001  | 18010      |
| edx-ora      | 3033  | 18091      |
| discern      | 7999  | 7999       |
| xserver      | 3031  |            |
| xqueue       | 3032  |            |

## 2. edX Analytics


| server	        | dev	| production |
|:----------------------|:------|:-----------|
| analytics-dashboard	| 9000	|            |
| analytics-data-api	| 9022	|            |
