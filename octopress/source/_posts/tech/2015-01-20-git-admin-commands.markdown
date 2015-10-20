---
layout: post_tech
title: "Git Admin Commands"
date: 2015-01-20 18:16:36 +0800
comments: true
categories: [tech]
tags: [git]
toc: false
---

## 1. Git

```bash
$ sudo apt-get install git-core

$ git config --global user.email "you@example.com" 
$ git config --global user.name "username" 
```

## 2. Create repos

```bash
sudo su git
cd
cd repositories

mkdir -p edx-analytics-pipeline.git
git init --bare edx-analytics-pipeline.git
cd edx-analytics-pipeline.git
git config core.sharedRepository true
cat config
cd ..

exit
```

## 3. Manage projects

```bash
$ cd /gitadmin
$ cd gitolite-admin
$ sudo nano conf/gitolite.conf
```

gitolite.conf 

```bash
repo    gitolite-admin
        RW+     =   insight

repo    testing
        RW+     =   @all

#---------------------------------------------------#
# edx-insight

@edxgroup = insight105

repo    edx-analytics-pipeline
        RW+     =   @edxgroup

repo    edx-analytics-data-api
        RW+     =   @edxgroup

repo    edx-analytics-dashboard
        RW+     =   @edxgroup
```

config users

```bash
$ cat ~/.ssh/id_rsa.pub
$ cd /gitadmin/gitolite-admin/keydir
$ nano username.pub                    # copy the public key here

$ cd /gitadmin/gitolite-admin

$ git add conf/
$ git commit -am "assigned the edx repos to the user insight105" 
$ git push origin master

$ git add keydir/
$ git commit -am "added the user insight105" 
$ git push origin master
```
