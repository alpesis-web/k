---
layout: post_tech
title: "Installing edX Analytics Data API Client"
date: 2014-11-02 12:43:09 +0800
comments: true
categories: [tech]
tags: [edX, edX Analytics, python]
toc: false
---

## 1. Create the project

```bash
$ sudo mkdir analytics
$ sudo mkdir analytics/app

$ cd analytics
$ sudo virtualenv venv
$ source venv/bin/activate

$ deactivate  # exit
```

## 2. Download the project

```bash
$ cd analytics/app
$ sudo git clone https://github.com/edx/edx-analytics-data-api-client.git
```

## 3. Install the project

```bash
$ cd edx-analytics-data-api-client
$ sudo make validate
```
