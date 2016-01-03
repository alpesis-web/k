---
layout: post_tech
title: "Vim Commands"
date: 2014-01-18 19:34:30 +0800
comments: true
categories: [tech]
tags: [vim, ide]
---

- normal mode: to edit a file
- command mode: to edit a file with commands

## 1. Normal Mode


| Type                  | Action                | Command                                      |
|:----------------------|:----------------------|:---------------------------------------------|
| edit                  | insert                | i                                            |
|                       | delete                | x                                            |
|                       | exit                  | esc                                          |
| move                  | left, right, down, up | h, l, j, k                                   |
|                       | back word             | b                                            |
|                       | next word             | w                                            |
|                       | bebin of line         | ^                                            |
|                       | end of line           | $                                            |
|                       | jump to line col      | (col)|                                       |
|                       | page up               | ctrl + u                                     |
|                       | page down             | ctrl + d                                     |
|                       | top of the file       | gg                                           |
|                       | end of the file       | shift + g                                    |
| copy, paste, change   | yank, copy            | y(xxx)                                       |
|                       | cureent line          | yy                                           |
|                       | paste                 | p                                            |
|                       | change                | c                                            |
|                       | change word           | cw                                           |
| search, find, replace | search                | /(word)                                      |
|                       | search to top         | #                                            |
|                       | search to end         | *                                            |
|                       | find                  | f(a letter)                                  |
|                       | find former           | ,                                            |
|                       | find next             | ;                                            |
| delete                | delelte a line        | dd                                           |
| fold                  | fold                  | zc                                           |
|                       | unfold (single)       | zo                                           |
|                       | unfold (all)          | zr                                           |


Note: all for editing.

## 2. Command Mode

| Type                  | Action                | Command                                      |
|:----------------------|:----------------------|:---------------------------------------------|
| coding                | shift                 | :line,line< or :line,line>                   |
| files                 | save                  | :w                                           |
|                       | open                  | :e                                           |
| buffer                | switch                | :b<n>                                        |
|                       | close                 | :bd                                          |
| copy, replce          | copy paragraph        | :<line,line> copy <line,line>                |
|                       | replace               | :%s/(words)/(words replaced)/g, g for global |

