---
layout: post_tech
title: "Installing edX Analytics Data API"
date: 2014-11-02 13:44:48 +0800
comments: true
categories: [tech]
tags: [edX, edX Analytics]
---

## 1. Install in a single server

### 1.1. Download the repo

```bash
$ mkdir -p edx
$ cd edx
$ git clone https://github.com/edx/edx-analytics-data-api.git
```

### 1.2. Install the requirements

```bash
$ cd edx-analytics-data-api
$ make develop
```

### 1.3. Testing

```bash
$ ./manage.py migrate --noinput
$ ./manage.py migrate --noinput --database=analytics
$ ./manage.py set_api_key <username> <token>
$ make loaddata
$ ./manage.py runserver
```

link: http://localhost:9001


## 2. Install by ansible scripts

### 2.1. Download configuration

```bash
$ mkdir edx
$ cd edx
$ git clone https://github.com/edx/configuration.git
$ pip install -r requirements.txt
```

### 2.2. Install edx-analytics-data-api

```bash
$ cd configuration/playbooks
$ sudo ansible-playbook -c local ./run_role.yml -e "analytics-api"  -i "localhost,"
```

### 2.3. Testing

```bash
$ cd /edx/edxapp/edx-analytics-api
$ source venvs/edx-analytics-api/bin/activate
$ cd edx-analytics-api
$ make develop
$ ./manage.py migrate --noinput
$ ./manage.py migrate --noinput --database=analytics
$ ./manage.py set_api_key <username> <token>
$ make loaddata
$ ./manage.py runserver 0.0.0.0:9001
```

## References

- [configuration](https://github.com/edx/configuration/)
- [edx-analytics-data-api](https://github.com/edx/edx-analytics-data-api)
