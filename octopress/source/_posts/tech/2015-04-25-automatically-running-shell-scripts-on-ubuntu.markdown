---
layout: post_tech
title:  "Automatically Running Shell Scripts on Ubuntu"
date:   2015-04-25 14:17:14 +0800
comments: true
categories: [tech]
tags: [linux, ubuntu]
---

configurating script

```bash
$ cd /path/to/folder
$ nano file_name
# save and exit

$ sudo chown user:group file_name
$ sudo chmod u+x file_name
```

editting shell script

```bash
#!/bin/bash
...
...
```

configurating env path

```bash
$ nano ~/.bashrc

# custom bin
# export PATH=$PATH:/path/to/bin

$ source ~/.bashrc
```
