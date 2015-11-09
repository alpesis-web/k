---
layout: post_tech
title: "Generating A Self-Signed SSL Certificate"
date: 2015-11-09 22:59:53 +0800
comments: true
categories: [tech]
tags: [web, ssl]
toc: false
---

## Prequistion

openssl

```bash
$ which openssl

# OS X
$ brew install openssl

# Ubuntu
$ sudo apt-get install openssl
```

## 1. Private key and certificate signing request

```bash
$ openssl genrsa -des3 -passout pass:x -out server.pass.key 2048
$ openssl rsa -passin pass:x -in server.pass.key -out server.key
$ rm server.pass.key
$ openssl req -new -key server.key -out server.csr
```

## 2. SSL Certificate

```bash
$ openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```
