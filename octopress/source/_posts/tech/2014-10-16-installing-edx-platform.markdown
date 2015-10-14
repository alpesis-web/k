---
layout: post_tech
title: "Installing edX Platform"
date: 2014-10-16 13:08:29 +0800
comments: true
categories: [tech]
tags: [edX, edX Platform]
---

## 1. Devstack

### 1.1. Prequsition

- Vagrant
- VirtualBox
- Ubuntu 12.04


### 1.2. Download box

download Vagrantfile

```bash
$ mkdir -p edx/devstack
$ cd edx/devstack
$ curl -L https://raw.githubusercontent.com/edx/configuration/master/vagrant/release/devstack/Vagrantfile > Vagrantfile
$ vim Vagrantfile
```

Look for the url of the specific box, download it, and then `vagrant box add ...`

```bash
$ vagrant box add box_name box_path/box_name
$ vagrant box list
```

### 1.3. Create edX VM

```bash
$ vagrant plugin install vagrant-vbguest
$ vagrant up
```

NOTE: It will cost a lot of time on downloading and provisioning, please make sure the network connection is good.


### 1.4. Testing

```bash
$ vagrant ssh
$ sudo su edxapp
$ paver devstack lms
$ paver devstack studio
```

links:

- LMS: http://localhost:8000
- CMS/Studio: http://localhost:8001

accounts:

```
staff@example.com (password: edx)
audit@example.com (password: edx)
verify@example.com (password: edx)
honor@example.com (password: edx)
```


## 2. Fullstack

### 2.1. Fullstack

```bash
$ mkdir fullstack
$ cd fullstack
$ curl -L https://raw.githubusercontent.com/edx/configuration/master/vagrant/release/fullstack/Vagrantfile > Vagrantfile
$ vagrant plugin install vagrant-hostsupdater
$ vagrant up
```

### 2.2. Testing

Links:

- LMS: http://192.168.33.10/
- Studio: http://192.168.33.10:18010/

Accounts:

```
staff@example.com (password: edx)
audit@example.com (password: edx)
verify@example.com (password: edx)
honor@example.com (password: edx)
```

## References

- [edX Developer Stack](https://github.com/edx/configuration/wiki/edX-Developer-Stack)
- [edx Full stack installation using Vagrant Virtualbox](https://github.com/edx/configuration/wiki/edx-Full-stack--installation-using-Vagrant-Virtualbox)
