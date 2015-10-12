---
layout: post_tech
title:  "Installing PyCharm on Ubuntu 12.04"
date:   2015-04-25 10:08:14 +0800
comments: true
categories: [tech]
tags: [pycharm, ubuntu, linux]
---


### Installing Java

```bash
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-java7-installer
```

### Installing PyCharm

```bash
$ mkdir -p ~/opt/packages/pycharm
$ cd ~/opt/packages/pycharm
$ wget http://download.jetbrains.com/python/pycharm-community-4.0.4.tar.gz

$ gzip -dc pycharm-community-4.0.4.tar.gz | tar xf -
$ ln -s ~/opt/packages/pycharm/pycharm-community-4.0.4 ~/opt/pycharm

# Start PyCharm
$ ~/opt/pycharm/bin/pycharm.sh
```
