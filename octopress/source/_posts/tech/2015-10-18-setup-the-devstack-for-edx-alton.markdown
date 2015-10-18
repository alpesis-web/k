---
layout: post_tech
title: "Setup the devstack for edX Alton"
date: 2015-10-18 16:24:54 +0800
comments: true
categories: [tech]
tags: [edX, edX AI, hipchat, chatbot, robotics, will, docker]
toc: true
---

## Prequisitions

- Ubuntu 14.04
- Docker
- Git

## 1. Installation 

```bash
$ cd /path/to/project
$ git clone https://github.com/edx/alton.git
$ cd althon
$ sudo docker build -t alton .
```

Once done, check the image with `docker images`

```bash
vagrant@vagrant-ubuntu-trusty-64:/tmp/alton$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
alton               latest              367fc778d527        29 seconds ago      1.084 GB
python              2.7.7               a87a2288ce78        15 months ago       1.043 GB
```

## References

- [alton](https://github.com/edx/alton)
