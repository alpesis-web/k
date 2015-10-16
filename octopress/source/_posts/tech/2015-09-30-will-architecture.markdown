---
layout: post_tech
title: "Will Architecture"
date: 2015-09-30 15:19:59 +0800
comments: true
categories: [tech]
tags: [will, python, chatbot, robotics]
---

## 1. Features

## 2. Architect

## 3. File Construct

```
./
 |
 |---- will/                                    # src
 |       |---- mixins/
 |       |---- plugins/                         # extensions
 |       |---- scripts/                         # commands
 |       |---- storage/                         # dbs
 |       |---- templates/                       # html
 |       |---- tests/                           # testing
 |       |
 |       |---- acl.py
 |       |---- decorators.py
 |       |---- listener.py
 |       |---- plugin.py
 |       |---- scheduler.py
 |       |---- utils.py
 |       |
 |       |---- settings.py
 |       |---- main.py
 |
 |---- config.py                                # config file
 |---- run_will.py                              # run will
```

