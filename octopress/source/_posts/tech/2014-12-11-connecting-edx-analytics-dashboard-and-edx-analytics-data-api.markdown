---
layout: post_tech
title: "Connecting edX-Analytics-Dashboard and edX-Analytics-Data-API"
date: 2014-12-11 12:36:43 +0800
comments: true
categories: [tech]
tags: [edX, edX Analytics, python]
toc: false
---

## 1. edX Analytics Data API

```bash
$ python manage.py runserver 0.0.0.0:9022
```

## 2. edX Analytics Dashboard

/path/edx-analytics-dashboard/analytics_dashboard/settings/base.py

```python base.py
########## DATA API CONFIGURATION
#DATA_API_URL = 'http://127.0.0.1:9001/api/v0'
DATA_API_URL = 'http://127.0.0.1:9022/api/v0'
DATA_API_AUTH_TOKEN = 'edx'
########## END DATA API CONFIGURATION
```

run service

```bash
$ python manage.py runserver 0.0.0.0:9000
```
