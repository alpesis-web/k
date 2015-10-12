---
layout: post_tech
title: "It Could Not Access Network on Ubuntu"
date: 2015-04-25 14:08:20 +0800
comments: true
categories: [tech]
tags: [linux, ubuntu, network]
---

## 1. Interfaces

```bash
$ sudo nano /etc/network/interfaces
```

/etc/network/interfaces

```
auto lo
iface lo inet loopback

auto eth1
iface eth1 inet static
address 192.168.2.250
netmask 255.255.255.0
gateway 192.168.2.1
```

## 2. DNS

```bash
$ sudo nano /etc/resolv.conf
```

/etc/resolv.conf

```
# Google
nameserver 8.8.8.8
nameserver 8.8.4.4

# or
# OpenDNS
nameserver 208.67.222.222
nameserver 208.67.220.220
```

## 3. Testing


```bash
$ sudo /etc/init.d/networking restart
```
