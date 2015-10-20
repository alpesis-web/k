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
| node.js | npm             |                 | nvm             |
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

nvm

```bash
$ sudo git clone https://github.com/creationix/nvm.git ~/.nvm
$ cd ~/.nvm
$ sudo git checkout v0.17.0
$ sudo chmod 777 ~/.nvm
$ source ~/.nvm/nvm.sh

# install node.js
$ nvm install 0.10.34

# install npm
$ sudo apt-get install npm
```

install without nvm

```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo reboot

$ sudo apt-get install curl
$ sudo curl -sL https://deb.nodesource.com/setup | sudo bash -
$ sudo apt-get install nodejs

$ sudo apt-get install build-essential

$ node -v
$ npm -v
```

### 5.2. Commands

```bash
$ npm install PACKAGE

$ npm config set registry http://registry.cnpmjs.org // change source
$ npm info express  // package info

$ npm --registry http://registry.cnpmjs.org info express

$ nano ~/.npmrc   // config npm
$ registry =https://registry.npm.taobao.org   // config source
```

### 5.3. Configuration


### 5.4. Sources

```
http://registry.cnpmjs.org
https://registry.npm.taobao.org
```


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
