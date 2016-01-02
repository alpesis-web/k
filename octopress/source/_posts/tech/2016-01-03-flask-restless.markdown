---
layout: post_tech
title: "Flask-Restless"
date: 2016-01-03 00:09:13 +0800
comments: true
categories: [tech]
tags: [python, flask, rest, api]
toc: true
---

Source Codes:

- original: https://github.com/jfinkels/flask-restless
- refactored: https://github.com/KellyChan/flask-restless


Notes:
Flask-restless was developed by jfinkels, this article is the study notes
of his source code, and refactoring some codes by personal purpose.

All the commands and codes below are based on the [refactored codes][1].

  [1]: https://github.com/KellyChan/flask-restless


## Setup DevEnv

```bash
$ git clone https://github.com/KellyChan/flask-restless
$ cd flask-restless

# update Vagrantfile (Virtualbox + Vagrant)
# then create the project and setup the virtualenv
$ virtualenv venvs/flask-restless
$ source venvs/flask-restless/bin/activate

# install requirements
$ pip install -r requirements/base.txt
$ pip install -r requirements/test.txt
$ pip install -r requirements/docs.txt
```

## Project Structure

```bash
Dev
  |---- requirements/
  |---- flask_restless/
  |---- artwork/

DevOps
  |---- setup.cfg
  |---- setup.py
  |---- scripts/

Tests
  |---- tests/

Docs
  |---- docs/
  |---- examples/

Others
  |---- AUTHORS
  |---- licenses/
  |---- CHANGES
  |---- README.md
```

## Dev

files

```bash
flask-restless/
    |---- helpers.py                   # utils, mostly about SQLalchemy objects
    |---- manager.py                   # API creator
    |---- search.py                    # seraching database
    |---- views.py                     # views
```

## Test

### cli

```bash
$ nosetests
$ python setup.py test
$ nosetests --with-coverage --cover-package=flask_restless --cover-html \
    --cover-html-dir=<somedir>
$ savalidation
```

## Docs

### cli

```bash
$ git submodule update --init
$ python setup.py develop
$ cd docs
$ make html
$ sphinx-autobuild ./ _build/html/
```

### theming

## DevOps

installation

```bash
$ python setup.py --help
$ python setup.py install
```

## References
