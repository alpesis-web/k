---
layout: post_tech
title: "Git Commands"
date: 2016-01-08 18:06:55 +0800
comments: true
categories: [tech]
tags: [git]
toc: true
---

- config
- remote
- pull/commit
- log

## Config

```bash
$ git config --global user.name 'xxx'
$ git config --global user.email 'xxx'
$ git config --local user.name 'xxx'
$ git config --local user.email 'xxx'

$ git config --global alias.logtree "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

## Remote

```bash
$ git remote add origin https://xxxx                    # set remote url
$ git remote set-url origin https://xxxx                # update remote url
```

## Pull

```bash
$ git pull --rebase origin branch_name
$ git fetch origin
$ git rebase origin
```

## Commit

```bash
$ git diff <file>
$ git add <file(s)>
$ git status
$ git reset HEAD <file>
$ git commit -m "xxx"
$ git push origin <branch_name>
```

## Log

```bash
$ git log
$ git show <id>
```
