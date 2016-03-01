---
layout: post_tech
title: "Setting Up DevEnv for Nao"
date: 2016-02-25 13:07:11 +0800
comments: true
categories: [tech]
tags: [nao, robotics, aldebaran, webots, choregraphe, naoqi]
toc: true
---

Summary:

- Nao OS
- Devkits
  - Choregraphe suite
  - Naoqi SDK
  - Simulators: webots
- VM
- TroubleShooting

## 1. Nao OS

## 2. Choregraphe suite

Linux/Mac/Windows

- Download the installation package, and install it in a specific os


## 3. Naoqi SDK

- Python SDK
- C++ SDK
- Java SDK

### 3.1. Python SDK

#### Ubuntu

```bash
# download the package
$ cd /tmp
$ wget http://xxxx      # package path
$ tar -xvf pynaoqi-python2.7-x.x.x.x-linux64
$ mv pynaoqi-python2.7-x.x.x.x-linux64 pynaoqi-sdk
$ mv pynaoqi-sdk /usr/local/pynaoqi-sdk
$ vim ~/.bashrc
###########################################
export PYTHONPATH=${PYTHONPATH}:/usr/local/pynaoqi-sdk
########################################### 
$ source ~/.bashrc
$ python
###########################################
import naoqi
###########################################
```

#### Mac

```bash
# download the package
$ cd /tmp
$ wget http://xxxx      # package path
$ tar -xvf pynaoqi-python2.7-x.x.x.x-linux64
$ mv pynaoqi-python2.7-x.x.x.x-linux64 pynaoqi-sdk
$ mv pynaoqi-sdk /usr/local/pynaoqi-sdk
$ vim ~/.bash_profile
###########################################
export PYTHONPATH=${PYTHONPATH}:/usr/local/pynaoqi-sdk
export DYLD_LIBRARY_PATH=${DYLD_LIBRARY_PATH}:/path/to/python-sdk
########################################### 
$ source ~/.bash_profile
$ python
###########################################
import naoqi
###########################################
```

#### Windows

### 3.2. C++ SDK

#### Ubuntu

#### Mac

#### Windows

### 3.3. Java SDK

## 4. Simulators

### 4.1. Webot

- Download the specific installation package
- Install
- Register a license
- Update the packages
- Go to the dashboard

## 5. VM

- Download the box file
- Load the file into VirtualBox
- Login the OS with user/pass: nao/nao

## 6. TroubleShooting

## References

- [Aldebaran Softwares](https://community.aldebaran.com/en/resources/software/language/en-gb)
