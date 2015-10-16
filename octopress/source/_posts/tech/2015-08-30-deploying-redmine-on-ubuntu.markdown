---
layout: post_tech
title: "Deploying Redmine on Ubuntu"
date: 2015-08-30 23:46:36 +0800
comments: true
categories: [tech]
tags: [ubuntu, linux, redmine, mysql, apache]
---

## Requirements

- Ubuntu 12.04/14.04
- Redmine
- MySQL
- Apache2

## 1. MySQL

```bash
sudo apt-get install mysql-server mysql-client
```

## 2. Redmine

```bash
$ sudo apt-get install redmine redmine-mysql libapache2-mod-passenger
$ sudo dpkg-reconfigure redmine
```

## 3. Apache2

to link redmine to apache2

```bash
$ sudo ln -s /usr/share/redmine/public/ /var/www/redmine
```

edit passenger.conf

```
$ sudo nano /etc/apache2/mods-available/passenger.conf

# add 

PassengerDefaultUser www-data
```

edit sites-available//default

```bash
$ sudo nano /etc/apache2/sites-available/default

# add

<Directory /var/www/redmine>
RailsBaseURI /redmine
PassengerResolveSymlinksInDocumentRoot on
</Directory>
```

restart apache2

```bash
$ sudo service apache2 restart
```

## 4. Testing

- Site: http://ip_address/redmine
- Username/Password: admin/admin
