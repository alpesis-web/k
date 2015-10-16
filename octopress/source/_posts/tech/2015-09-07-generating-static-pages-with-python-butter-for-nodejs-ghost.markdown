---
layout: post_tech
title: "Generating Static Pages with Python Butter for NodeJS Ghost"
date: 2015-09-07 22:09:00 +0800
comments: true
categories: [tech]
tags: [python, butter, ghost, nodejs]
---

## 1. Installing Dependencies

```bash
$ sudo apt-get install libxml2-dev libxslt1-dev
```

Without the above dependencies, it will flag error:

```bash
pip instal buster: lxml error: command 'gcc' failed with exit status 1
``` 

## 2. Installing Butter

```bash
$ cd /private/apps/diary/
$ sudo mkdir venvs

$ sudo apt-get install python-virtualenv
$ sudo virtualenv venvs/diary
$ source venvs/diary/bin/activate

$ cd /private/apps/diary/diary
$ sudo mkdir static_pages
$ cd static_pages
$ sudo mkdir requirements
$ sudo nano requirements/base.txt    # add buster==0.1.3
$ sudo pip install -r requirements/base.txt

$ cd /private/apps/diary/diary/static_pages
$ buster generate --domain=http://localhost:2368  
$ buster preview  
```

Site: http://localhost:9000

## 3. Config Bin

```bash
$ cd /atr/apps/atr-blog/atr-blog/buster

$ sudo mkdir bin
$ sudo vim requirments.sh       # add sudo apt-get install libxml2-dev libxslt1-dev
```

## 4. Uploading to Github Pages

```bash
$ rm -rf static
$ git clone https://github.com/username/project.git static

$ buster generate --domain=http://localhost:2233
$ buster preview  
$ # open http://localhost:9000

$ buster deploy   
```
