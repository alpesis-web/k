---
layout: post_tech
title: "Installing edX Analytics Pipeline"
date: 2014-12-17 14:01:24 +0800
comments: true
categories: [tech]
tags: [edX, edX Analytics]
toc: false
---

## Requirements

- Python 2.7.x
- GCC
- MySQL
- GnuPG 1.4.x

mysql

```bash
sudo apt-get install mysql-server mysql-client
```

## Installation

```bash
$ sudo mkdir analytics
$ sudo mkdir analytics/app

$ cd analytics
$ sudo virtualenv analytics_venv
$ source analytics_venv/bin/activate

$ # deactivate


$ cd analytics/app
$ sudo git clone https://github.com/edx/edx-analytics-pipeline.git
$ cd edx-analytics-pipeline 

$ make develop
$ sudo make test
```
