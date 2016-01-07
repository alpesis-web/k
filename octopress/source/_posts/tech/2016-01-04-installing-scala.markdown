---
layout: post_tech
title: "Installing Scala"
date: 2016-01-04 13:06:34 +0800
comments: true
categories: [tech]
tags: [scala, java]
toc: false
---

## Prequisition

Java

```bash
$ sudo add-apt-repository -y ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get -y install oracle-java8-installer
```

## Installation

### Ubuntu 14.04

```bash
$ cd /tmp
$ wget http://www.scala-lang.org/files/archive/scala-2.11.6.deb
$ sudo dpkg -i scala-2.11.6.deb
$ sudo apt-get update
$ sudo apt-get install scala
```

### OS X

```bash
$ brew update
$ brew install scala
$ brew install sbt
```

## Testing

```bash
vagrant@vagrant-ubuntu-trusty-64:/vagrant/scala$ scala
Welcome to Scala version 2.11.6 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_66).
Type in expressions to have them evaluated.
Type :help for more information.

scala> 1+1
res0: Int = 2

scala> println("Hello")
Hello

scala>:quit
```

## Vim Configuration

syntax highlight: [vim-scala](https://github.com/derekwyatt/vim-scala)

```bash
# wget
$ mkdir -p ~/.vim/{ftdetect,indent,syntax} && for d in ftdetect indent syntax ; do wget -O ~/.vim/$d/scala.vim https://raw.githubusercontent.com/derekwyatt/vim-scala/master/$d/scala.vim; done

# curl
$ mkdir -p ~/.vim/{ftdetect,indent,syntax} && for d in ftdetect indent syntax ; do curl -o ~/.vim/$d/scala.vim https://raw.githubusercontent.com/derekwyatt/vim-scala/master/$d/scala.vim; done
```
