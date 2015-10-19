---
layout: post_tech
title: "Installing edX Analytics Dashboard"
date: 2014-11-01 13:29:38 +0800
comments: true
categories: [tech]
tags: [edX, edX Analytics, ansible]
---

## 1. Install in a single server

### 1. Prequistions

- Python 2.7.x (not tested with Python 3.x)
- gettext
- node.js > 0.10.x
- npm
- JDK 7+

### 2. Download the repo

```bash
$ mkdir -p edx
$ cd edx
$ git clone https://github.com/edx/edx-analytics-dashboard.git
```

### 3. Install the requirements

install by manually

```bash
$ cd edx-analytics-dashboard
$ pip install -r requirements.txt
$ bundle install
$ bower install
```

install by commands

```bash
$ make develop
```

Here there are 3 steps:

- node.js: `npm install package`
- bower: `bower install`
- python: `python install -r requirements/xxx.txt`

### 4. Testing

```bash
$ make migrate
$ ./manage.py runserver 0.0.0.0:9000
```


## 2. Install with ansible scripts

### 2.1. Download configuration

```bash
$ mkdir edx
$ cd edx
$ git clone https://github.com/edx/configuration.git
$ pip install -r requirements.txt
```

### 2.2. Install edx-analytics-dashboard

```bash
$ cd configuration/playbooks 
$ sudo ansible-playbook -c local ./run_role.yml -e "insights"  -i "localhost,"
``` 

### 2.3. Testing

```bash
$ cd /edx/edxapp/insights
$ source venvs/insights/bin/activate
$ cd edx-analytics-dashboard
$ make migrate
$ ./manage.py runserver 0.0.0.0:9000
```

site: http://localhost:9000/test/auto_auth/

## References

- [configuration](https://github.com/edx/configuration/)
- [edx-analytics-dashboard](https://github.com/edx/edx-analytics-dashboard/)
