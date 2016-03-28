---
layout: post_tech
title: "GIL in Python"
date: 2016-03-15 17:57:57 +0800
comments: true
categories: [tech]
tags: [python, programming, threading]
toc: true
---

## GIL

GIL: Global Interpreter Lock

- it limits thread performance

## GIL and Python Threads

GIL

- parallel execution is forbidden
- there is a "global interpreter lock"
- the GIL ensures that only one thread runs in the interpreter at once
- simplified many low-level details (memory management, callouts to C extensions, etc.)


Python Threads

- Python threads are real system threads
- fully managed by the host operating system
- represent threaded execution of the Python interpreter process (written in C)

Thread execution model


## References

- [Understanding the Python GIL](http://www.dabeaz.com/python/UnderstandingGIL.pdf)
