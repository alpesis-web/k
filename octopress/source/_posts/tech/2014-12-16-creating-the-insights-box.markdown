---
layout: post_tech
title: "Creating The Insights Box"
date: 2014-12-16 13:46:06 +0800
comments: true
categories: [tech]
tags: [edX, edX Analytics, vagrant, virtualbox]
toc: true
---

This tutorial is about creating the insights box for the development. 
Once the box is created, and then you can import it again to other machines.

## Requirements

- precise64.box
- Vagrant
- VirtualBox
- edx-analytics-data-api
- edx-analytics-dashboard

## 1. Vagrant / VirtualBox

```bash
$ vagrant box add precise64 precise64.box

$ mkdir edx-analytics
$ cd edx-analytics
$ vagrant init precise64
$ vagrant up
```

## 2. Precise64

```bash
$ vagrant ssh

$ sudo apt-get update
$ sudo apt-get upgrade

$ sudo apt-get install npm
$ sudo apt-get install node

$ sudo apt-get install python-virtualenv
$ sudo apt-get install python-dev

$ sudo apt-get install libevent-dev
$ sudo apt-get install build-essentia

$ sudo pip install -U distribute
$ sudo apt-get install mysql-server
```

## 3. Projects

```bash
$ sudo mkdir -p /analytics
$ sudo mkdir -p /analytics/app

$ cd /analytics/app
$ sudo mkdir -p /analytics/app/edx-analytics-data-api
$ sudo mkdir -p /analytics/app/edx-analytics-dashboard

$ cd /analytics/app/edx-analytics-data-api
$ sudo git clone https://github.com/edx/edx-analytics-data-api
$ sudo mv edx-analytics-data-api app/

$ cd /analytics/app/edx-analytics-dashboard
$ sudo git clone https://github.com/edx/edx-analytics-dashboard
$ sudo mv edx-analytics-dashboard/app

$ sudo chown -R vagrant:vagrant /analytics
$ sudo chmod 750 /analytics
```

## 4. Configurations

### 4.1. edx-analytics-data-api

```bash
$ cd edx-analytics-data-api
$ sudo virtualenv api_env
$ source api_env/bin/activate

$ cd app
$ make develop

$ ./manage.py migrate --noinput
$ ./manage.py migrate --noinput --database=analytics
$ ./manage.py set_api_key <username> <token>
$ ./manage.py runserver 0.0.0.0:9001
```

### 4.2. edx-analytics-dashboard

```bash
$ cd edx-analytics-dashboard
$ sudo virtualenv board_env
$ source board_env/bin/activate

$ cd app
$ make develop
$ make migrate
$ cd analytics_dashboard
$ ./manage.py runserver 0.0.0.0:9000
```

## 5. Export Box

```bash
$ vagrant package --base <vm_id>

# for example
$ vagrant package --base vagrant_default_1404223024566_2093
```


## 6. Import Box

### 6.1. create a vm

```bash
$ vagrant box list
$ vagrant box add insights-devstack insights-devstack.box

$ cd /path/edx-analytics/insights-devstack
$ vagrant up
```

### 6.2. edx-analytics-data-api

```bash
$ cd /analytics/app/edx-analytics-data-api
$ source api_env/bin/activate
$ cd app

$ python manage.py migrate --noinput
$ python manage.py migrate --noinput --database=analytics

# Create a user and authentication token. Note that the user will be created if one does not exist.
$ python manage.py set_api_key edx edx

$ make loaddata
$ python manage.py runserver 0.0.0.0:9001
```

site: http://localhost:9001


### 6.3. edx-analytics-dashboad


```bash
$ cd /analytics/app/edx-analytics-dashboard
$ source board_env/bin/activate
$ cd app

$ sudo make migrate
$ cd analytics_dashboard
$ sudo python manage.py runserver 0.0.0.0:9000
```

sites:

- http://localhost:9000
- http://localhost:9000/test/auto_auth/



