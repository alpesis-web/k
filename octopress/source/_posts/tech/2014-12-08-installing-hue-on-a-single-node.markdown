---
layout: post_tech
title: "Installing Hue on A Single Node"
date: 2014-12-08 09:48:12 +0800
comments: true
categories: [tech]
tags: [hue, hadoop, cluster]
toc: true
---

## Requirements

- Vagrant/VirtualBox
- Ubuntu 12.04

## 1. Vagrantfile

add ports

```ruby Vagrantfile
config.vm.network :forwarded_port, guest: 9000, host: 9000
```

## 2. Ubuntu

add user

```bash
$ vagrant ssh
$ cd ../..

$ sudo mkdir hadoop
$ sudo mkdir hadoop/apps

$ sudo chown -R vagrant:vagrant /hadoop
```

## 3. Hue

```bash
$ git clone http://github.com/cloudera/hue.git
$ cd hue
$ make apps
```

## 4. Testing

```bash
$ build/env/bin/hue runserver 0.0.0.0:9000
```

## Reference

- [Hue](https://github.com/cloudera/hue)
