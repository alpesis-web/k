---
layout: post_tech
title: "Installing Octopress"
date: 2015-10-06 03:54:31 +0800
comments: true
categories: [tech]
tags: [ruby, octopress]
---

## 1. Prequisitions

- Ubuntu 14.04
- Node.js
- RVM
- Ruby 2.1.1
- Gem
- Bundler
- Rake

Node.js

```bash
$ sudo apt-get install nodejs
```

RVM and Ruby

```bash
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
$ \curl -sSL https://get.rvm.io | bash -s stable
$ rvm install 2.1.1
```

## 2. Octopress

```bash
$ git clone git://github.com/imathis/octopress.git octopress
$ cd octopress
$ gem install bundler
$ bundle install
$ rake install
```

## 3. Testing

```bash
$ rake generate
$ rake preview
```

link: http://localhost:4000


## References

1. [Octopress](http://octopress.org/docs/setup/)
2. [Octopress Source Code](https://github.com/imathis/octopress)
