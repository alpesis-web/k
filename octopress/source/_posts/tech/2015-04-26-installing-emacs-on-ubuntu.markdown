---
layout: post_tech
title: "Installing Emacs on Ubuntu"
date: 2015-04-26 23:48:00 +0800
comments: true
categories: [tech]
tags: [emacs, ubuntu]
---

## Prequistions

```bash
$ sudo apt-get install ipython
$ sudo apt-get install texinfo
$ sudo apt-get install automake autoconf
$ sudo apt-get install texlive
$ sudo apt-get install cvs
$ sudo apt-get install mercurial
```

## 1. Installation

```bash
$ sudo add-apt-repository ppa:cassou/emacs
$ sudo apt-get update

$ # stable version
$ sudo apt-get install emacs24 emacs24-el emacs24-common-non-dfsg

$ # latest version
$ sudo apt-get install emacs-snapshot-el emacs-snapshot-gtk emacs-snapshot
```

## 2. Configuration

```bash
$ git clone https://github.com/jhamrick/emacs.git
 
$ cd emacs
$ sudo /path/to/emacs/bootstrap.sh

$ # open the emacs, it will automatically install the plugins    
$ emacs
```

## 3. Trouble Shooting

### 3.1. error: makeinfo: command not found

error message

```bash
$ error: makeinfo: command not found
```

solution

```bash
$ sudo apt-get install texinfo
```

### 3.2. error: autogen.sh: autoconf: not found

error message

```bash
error: autogen.sh: autoconf: not found
```

solution

```bash
$ sudo apt-get install automake autoconf
```

### 3.3. error: pdftex: command not found

error message

```bash
error: pdftex: command not found
```

solution

```bash
$ sudo apt-get install texlive
```

### 3.4. error: Error (el-get): while installing matlab-mode: The command named ‘cvs’ can not be found with `executable-find’

error message

```bash
error: Error (el-get): while installing matlab-mode: The command named ‘cvs’ can not be found with `executable-find’
```

solution

```bash
$ sudo apt-get install cvs
```

### 3.5. error: Error (el-get): while installing pydoc-info: The command named ‘hg’ can not be found with `executable-find’

error message

```bash
error: Error (el-get): while installing pydoc-info: The command named ‘hg’ can not be found with `executable-find’
```

solution

```bash
$ sudo apt-get install mercurial
```
