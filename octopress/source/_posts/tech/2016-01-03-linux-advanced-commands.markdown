---
layout: post_tech
title: "Linux Advanced Commands"
date: 2016-01-03 22:20:08 +0800
comments: true
categories: [tech]
tags: [linux, shell]
toc: false
---

## mkdir

```bash
$ mkdir -p project/{bin,venvs,app/{requirements,app/tests,docs,scripts}}
```

## tar

```bash
$ tar xvf -C /path/to/dest xxx.tar.gz
$ cd /path/to/dest && tar xvf xxx.tar.gz
$ cd /path/to/dest || mkdir -p /path/to/dest

$ cd /path/to/dest || mkdir -p /path/to/dest && tar xvf -C /path/to/dest xxx.tar.gz

$ cd /path/to/dest || \
> mkdir -p /path/to/dest && \
> tar xvf -C /path/to/dest xxx.tar.gz

$ ( cd /path/to/dest || mkdir -p /path/to/dest && \
> VAR=$PWD; cd ~; tar xvf -C $VAR archive.tar ) | \
> mailx admin -S "Archive contents"

$ { cp $(VAR)a . && chown -R guest.guest a && \
> tar cvf newarchive.tar a; } | \
> mailx admin -S "New archive"
```

## patterns

### file name: xargs

```bash
$ find some-file-criteria some-file-path | xargs some-great-command-that-needs-filename-arguements
```

### file contents: grep

```bash
$ time grep and tmp/a/longfile.txt | wc -l             # multiple files
$ time grep -c and tmp/a/longfile.txt                  # single file

$ grep -o and tmp/a/longfile.txt | wc -l
```

### content lines: awk

```bash
$ ls -la | awk '$6 == "Dec"'
```
