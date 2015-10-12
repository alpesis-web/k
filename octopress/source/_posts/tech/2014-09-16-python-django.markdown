---
layout: post_tech
title: "Python Django"
date: 2014-09-16 19:32:00 +0800
comments: true
categories: [tech]
tags: [python, django]
---

Project Construct

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

django app

```
polls/
    __init__.py
    admin.py
    models.py
    tests.py
    views.py
    urls.py

    migrations/
        __init__.py
    templates/
        appname/
            xxx.html
    static/
        appname/
            css/
            js/
            images/
```

## 1. MVC

### 1.1. Model

### 1.2. View

### 1.3. Control



## 2. Admin

## 3. Commands

```bash
$ django-admin.py startproject mysite

$ python manage.py migrate
$ python manage.py runserver
$ python manage.py runserver 8080
$ python manage.py runserver 0.0.0.0:8000
$ python manage.py startapp polls
$ python manage.py makemigrations polls
$ python manage.py sqlmigrate polls 0001
$ python manage.py migrate
$ python manage.py shell
$ python manage.py createsuperuser
$ python manage.py validate
$ python manage.py syncdb
$ python manage.py collectstatic
```
