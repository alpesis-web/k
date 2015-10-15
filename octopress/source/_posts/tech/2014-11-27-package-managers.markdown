---
layout: post_tech
title: "Package Managers"
date: 2014-11-27 12:52:01 +0800
comments: true
categories: [tech]
tags: [linux, nodejs, python, ruby, javascript] 
---

| SW      | Package Manager | Config File     | Version Control |
|:--------|:----------------|:----------------|:----------------|
| redhat  | rpm             |                 |                 |
| ubuntu  | apt             |                 |                 |
| python  | pip             | requirments.txt | virtualenv      |
| ruby    | gem             | Gemfile         | rvm             |
| node.js | npm             |                 |                 |
| js      | bower           | bower.json      |                 |

## 1. RedHat

## 2. Ubuntu

### 2.1. Installation

### 2.2. Commands

```bash
$ sudo apt-get update && upgrade
$ sudo apt-get install SW
```

### 2.3. Configuration

### 2.4. Sources

```bash
$ vim /etc/apt/sources.list
```

## 3. Python

### 3.1. Installation

```bash
$ sudo easy_install pip
```

### 3.2. Commands

```bash
$ pip install PACKAGE
$ pip install -r requirements.txt

$ pip uninstall PACKAGE
```

### 3.3. Configuration

```
$ vim requirements.txt

$ mkdir requirements
$ cd requirements
$ vim base.txt
$ vim dev.txt
$ vim production.txt
$ vim tests.txt
```

### 3.4. Sources

```bash
$ pip install PACKAGE -i http://xxxx
```

source list

## 4. Ruby

### 4.1. Installation

### 4.2. Commands

```bash
$ gem install PACKAGE
$ gem install -g PACKAGE

$ gem uninstall PACKAGE
```

### 4.3. Configuration

Gemfile

### 4.4. Sources


## 5. Node.js

### 5.1 Installation

### 5.2. Commands

### 5.3. Configuration


### 5.4. Sources


## 6. Javascript

### 6.1. Installation

```bash
$ npm install -g bower
```

### 6.2. Commands

```bash
$ bower install PACKAGE
```

### 6.3. Configuration

```bash
$ bower init
```

It will generate [bower.json](https://github.com/bower/spec/blob/master/json.md).

`bower.json`


### 6.4. Sources


## References

Linux

Python

Ruby

Node.js

Javascript

- [Bower Official Site](http://bower.io)
