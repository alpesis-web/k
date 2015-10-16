---
layout: post_tech
title: "Deploying NodeJS Ghost Production Stack"
date: 2015-09-05 22:19:58 +0800
comments: true
categories: [tech]
tags: [nodejs, ghost, git, nginx, forever, mysql]
---

## Prequisitions

- Ubuntu 12.04/14.04
- MySQL
- Node.js / NPM
- Ghost
- NPM / forever
- Nginx
- Git

## 1. Ubuntu

```bash
$ sudo apt-get install git
$ git config --global user.name "xxx" 
$ git config --global user.email "xxxx" 
``` 

## 2. MySQL

```bash
$ sudo apt-get install mysql-server mysql-client

$ mysql -uroot

mysql> create database ghost;
mysql> create database ghost_dev;
mysql> show schemas;
mysql> exit;
```

## 3. Node.js / NPM

```bash
$ sudo apt-get install --yes build-essential
$ sudo apt-get install curl
$ curl --silent --location https://deb.nodesource.com/setup_0.12 | sudo bash -
$ sudo apt-get install --yes nodejs
$ node -v
$ npm -v
```

## 4. Ghost

```
$ sudo su -
$ cd /
$ mkdir ghost
$ cd ghost
$ mkdir -p var/log
$ mkdir -p var/run
$ mkdir bin

$ sudo git clone https://github.com/username/project.git
$ git remote -v
$ git branch

$ sudo npm start --development
$ sudo npm start --production
```

- localhost:2368 
- localhost:2368/admin

## 5. Forever

```bash
$ sudo npm install -g forever

$ sudo node index.js
$ sudo forever start index.js

$ # sudo forever stop 0  
```

## 6. Nginx

```bash
$ sudo apt-get install nginx

$ cd /ghost
$ sudo mkdir -p etc/nginx
$ sudo ln ghost/config/nginx/diary.conf /ghost/etc/nginx/ghost.conf

$ sudo ln /ghost/etc/nginx/ghost.conf /etc/nginx/sites-available/ghost.conf
$ ls /etc/nginx/sites-available/

$ sudo ln /etc/nginx/sites-available/ghost.conf /etc/nginx/sites-enabled/ghost.conf
$ ls /etc/nginx/sites-enabled/

$ sudo service nginx restart
```

## 7. Auto Service Start

```bash
$ cd /ghost/ghost/config/bin
$ sudo cp diary-dev ghost              # update the paths

$ sudo ln ghost /ghost/bin/ghost
$ sudo ln /ghost/bin/ghost /etc/init.d/ghost
$ ls /etc/init.d/

$ sudo chmod a+x /etc/init.d/ghost
$ ls /etc/init.d/ghost
$ sudo update-rc.d ghost defaults
$ sudo service ghost start
```

ghost: check the paths

```bash
NAME="ghost" 
NODE_BIN_DIR="/usr/local/node/bin" 
NODE_PATH="/ghost/ghost/node_modules"           
APPLICATION_PATH="/ghost/ghost/index.js" 
PIDFILE="/ghost/var/run/ghost.pid"             
LOGFILE="/ghost/var/log/ghost.log"             
MIN_UPTIME="5000" 
SPIN_SLEEP_TIME="2000" 
```

## 8. Testing

- ip_address:8000
- ip_address:8000/admin
