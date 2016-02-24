---
layout: post_tech
title: "Installing ROS on Ubuntu"
date: 2016-02-20 21:04:48 +0800
comments: true
categories: [tech]
tags: [ubuntu, ros, ai, robotics]
toc: false
---

## Requirement

- Ubuntu 14.04

## ROS

### Installation

```bash
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
$ sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net:80 --recv-key 0xB01FA116
$ sudo apt-get update

$ sudo apt-get install ros-jade-desktop-full
```

### Configuration

```bash
$ sudo rosdep init
$ rosdep update

$ echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
$ source ~/.bashrc
``` 

### Additional Tools

```bash
$ sudo apt-get install python-rosinstall
```
