---
layout: post_tech
title: "Connecting edX MongoDB with MongoVUE and Putty"
date: 2014-11-20 16:50:54 +0800
comments: true
categories: [tech]
tags: [edX, mongo, mongovue]
toc: false
---

## 1. PuTTY

download [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
and install

Connection > SSH > Tunnels

<img src="https://s-media-cache-ak0.pinimg.com/736x/2a/a4/1a/2aa41ab3cb2208810ae64cb8b7dedfce.jpg" />

connection

- Source port: 5151
- Destination: 127.0.0.1:27017
- choose `local` and `IPv4`

<img src="https://s-media-cache-ak0.pinimg.com/736x/b6/77/5b/b6775bdd7ffef5ea1f7bffe69c776bda.jpg" />

save and login

<img src="https://s-media-cache-ak0.pinimg.com/736x/3b/57/b9/3b57b9b88b082f1dc1f14ef468b4496d.jpg" />

## 2. MongoVUE

- open MongoVUE: File -> Connect

<img src="https://s-media-cache-ak0.pinimg.com/736x/04/3c/a6/043ca63ca6fb153ee479893ea78a023d.jpg" />

connect

<img src="https://s-media-cache-ak0.pinimg.com/736x/fd/9a/2f/fd9a2ff48f498ceb7b00b9a7c831a55b.jpg" />

## Reference

- [Connecting to a remote server over SSH](http://www.mongovue.com/2011/08/04/mongovue-connection-to-remote-server-over-ssh/)
