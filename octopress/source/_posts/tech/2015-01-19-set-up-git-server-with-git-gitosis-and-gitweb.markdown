---
layout: post_tech
title: "Set Up Git Server with Git Gitosis and Gitweb"
date: 2015-01-19 15:33:49 +0800
comments: true
categories: [tech]
tags: [git, gitweb, gitosis, apache]
toc: true
---

# Server Side

## Requirements

- Ubuntu 12.04
- SSH
- Git
- Gitweb
- Gitosis
- Apache

## 1. Ubuntu

```bash
$ sudo apt-get update
$ sudo apt-get upgrade

$ ps -e | grep ssh
$ sudo apt-get install openssh-server
$ sudo /etc/init.d/ssh restart
```

## 2. Git

```bash
$ sudo apt-get install git-core
```

## 3. Gitosis

```bash
$ sudo apt-get install python-setuptools
$ cd /tmp
$ git clone https://github.com/tv42/gitosis.git
$ cd gitosis
$ sudo python setup.py install

$ sudo useradd -m git 
$ sudo passwd git

$ ssh-keygen -t rsa
$ sudo cp $HOME/.ssh/id_rsa.pub /tmp/
$ cd /tmp
$ sudo chmod 777 id_rsa.pub
$ sudo -H -u git gitosis-init < id_rsa.pub
$ sudo chmod 755 /home/git/repositories/gitosis-admin.git/hooks/post-update
```

## 4. Gitweb

```bash
$ sudo apt-get install gitweb

$ cd /var/www/
$ sudo ln -s /usr/share/gitweb/* .  # a space between * and . 
$ sudo nano /etc/gitweb.conf
```

gitweb.conf: update `$projectroot` as gitosis-admin.git

```
# path to git projects (<project>.git)
#$projectroot = "/var/cache/git";
$projectroot = "/home/git/repositories";
```

update authentication

```bash
$ sudo chmod 777 -R /home/git/repositories
```

## 5. Apache

```bash
$ sudo apt-get install apache2
```

NOTES:

- web default path: /var/www/
- cgi default path: /usr/lib/cgi-bin/

```bash
$ cd /usr/lib/cgi-bin/  # check if gitweb.cgi is here
```

restart apache

```bash
$ sudo /etc/init.d/apache2 restart
```

site: http://localhost/cgi-bin/gitweb.cgi

--- 

# Server Admin

```bash
$ ssh-keygen -t rsa

$ sudo apt-get install git-core

$ sudo mkdir -p /opt/git/directory.git
$ sudo git init --bare /opt/git/directory.git

$ sudo groupadd gitgroup
$ sudo chmod -R g+ws *
$ sudo chgrp -R gitgroup *

$ sudo usermod -G gitgroup jiansen

$ cd directory.git
$ sudo git config core.sharedRepository true


$ sudo adduser andy
$ sudo groupadd gitgroup
$ sudo usermod -G gitgroup andy

$ git config --global user.email "andy@example.com" 
$ git config --global user.name "andy" 

$ git clone andy@192.168.1.80:/opt/git/directory.git

$ cd directory
$ vim README
$ git add README
$ git commit -am 'fix for the README file by Andy'

$ git push origin master
```
