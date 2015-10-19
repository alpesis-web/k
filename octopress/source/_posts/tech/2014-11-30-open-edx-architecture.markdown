---
layout: post_tech
title: "Open edX Architecture"
date: 2014-11-30 16:03:01 +0800
comments: true
categories: [tech]
tags: [edX, edX Platform, edX Analytics]
toc: true
---

Open edX is a web-based platform for creating, delivering, and analyzing online courses.  It is the software that powers edx.org and many other online education sites.


Technologies

- server-side: `Python`, (web application framework) `Django`, (template) `Mako`
- browser-side: `Javascript`, `CoffeeScript`
- client-side: (framework) `Backbone.js`
- stylesheets: `Sass`, (framework) `Bourbon`
- dbs: `MySQL`, `MongoDB`

## 1. edX Platform

<img src="https://s-media-cache-ak0.pinimg.com/736x/c7/1b/ef/c71befac40bfd6a0ec6e8b719128cc0a.jpg" />

### 1.1. LMS

data

- courses: MongoDB
- videos: Youtube, Amazon S3
- student data: MySQL

events -> analytics pipeline

LTI provider

### 1.2. CMS

data

- courses: MongoDB

### 1.3. Course Browsing

- course homepage
- course discovery

### 1.4. Course Structure

XModules -> XBlocks

- LTI Tool: learning tools
- CodeJail: present the problem or assess the student’s response, python code
- JS Input: javascript components
- OLX: Open Learning XML, import and export courses

### 1.5. Discussions

- Comments Service: ruby, (framework) Sinatra
- Notifier: process that sends students notifications about updates in topics of interest

### 1.6. Mobile Apps

- ios
- android


### 1.7. Background Work

This work is queued and distributed using Celery and RabbitMQ. Examples of queued work include:

- Grading entire courses
- Sending bulk emails (with Amazon SES)
- Generating answer distribution reports
- Producing end-of-course certificates

### 1.8. Searching

Open edX uses Elasticsearch for searching in a few contexts: courseware search, the comments service, and the Student Notes feature.


## 2. edX Analytics


<img src="https://s-media-cache-ak0.pinimg.com/736x/e5/a7/d7/e5a7d79ffdfd8be033264afb4670aa79.jpg" />

Events describing student behavior are captured by the Open edX analytics pipeline.  The events are stored as JSON in S3, processed using Hadoop, and digested, aggregated results are published to MySQL. Results are made available via a REST API to Insights, a Django application that instructors and administrators use to explore data that lets them know what their students are doing and how their courses are being used.


## References

- [Open edX Architecture](https://open.edx.org/contributing-to-edx/architecture)
