---
layout: post_tech
title: "Python Sphinx"
date: 2015-12-31 01:25:59 +0800
comments: true
categories: [tech]
tags: [python, sphinx]
toc: true
---

Sphinx is a Python documentation generator. To write the project documentation with 
Sphinx, here are the steps:

1. Installing Sphinx and setting up a project
2. Configuring Sphinx
3. Customizing the theme
4. Writing the docs

## Sphinx Setup

Sphinx and plugins

- sphinx
- sphinx-autobuild: live html preview

### Installation

```bash
$ pip install sphinx
$ pip install sphinx-autobuild
```

### Quick Start

```bash
$ sphinx-quickstart

# sphinx-autobuild <DOCS_PATH> <DOCS_PATH/BUILD/HTML>
$ sphinx-autobuild docs docs/_build/html
```

other commands

```bash make.bat
if "%1" == "help" (
        :help
        echo.Please use `make ^<target^>` where ^<target^> is one of
        echo.  html       to make standalone HTML files
        echo.  dirhtml    to make HTML files named index.html in directories
        echo.  singlehtml to make a single large HTML file
        echo.  pickle     to make pickle files
        echo.  json       to make JSON files
        echo.  htmlhelp   to make HTML files and a HTML help project
        echo.  qthelp     to make HTML files and a qthelp project
        echo.  devhelp    to make HTML files and a Devhelp project
        echo.  epub       to make an epub
        echo.  latex      to make LaTeX files, you can set PAPER=a4 or PAPER=letter
        echo.  text       to make text files
        echo.  man        to make manual pages
        echo.  texinfo    to make Texinfo files
        echo.  gettext    to make PO message catalogs
        echo.  changes    to make an overview over all changed/added/deprecated items
        echo.  xml        to make Docutils-native XML files
        echo.  pseudoxml  to make pseudoxml-XML files for display purposes
        echo.  linkcheck  to check all external links for integrity
        echo.  doctest    to run all doctests embedded in the documentation if enabled
        echo.  coverage   to run coverage check of the documentation if enabled
        goto end
)
```

## Sphinx Settings

```python conf.py
# -- General configuration ------------------------------------------------
# -- Options for HTML output ----------------------------------------------
# -- Options for LaTeX output ---------------------------------------------
# -- Options for manual page output ---------------------------------------
# -- Options for Texinfo output -------------------------------------------
```

## Sphinx Theming

There are two options to create a theme: 

- one is to change a default theme,
- another is to create a new one by yourself.

### Default Themes

All html configurations are stored in `conf.py`. Official docs is [here][sphinx-theming].

  [sphinx-theming]: http://sphinx-doc.org/theming.html

```python conf.py
# -- Options for HTML output ----------------------------------------------

# The theme to use for HTML and HTML Help pages.  See the documentation for
# a list of builtin themes.
# default themes: alabaster, sphinx_rtd_theme, classic, sphinxdoc,
#     scrolls, agogo, traditional, nature, haiku, pyramid, bizstyle
html_theme = 'nature'

# Theme options are theme-specific and customize the look and feel of a theme
# further.  For a list of options available for each theme, see the
# documentation.
# html_theme_options = {
#     "rightsidebar": "true",
#     "relbarbgcolor": "black"
# }
html_theme_options = {}

# Add any paths that contain custom themes here, relative to this directory.
# html_theme_path = ["."]
html_theme_path = []
```

### Custom Themes

## RST

### Makeup Constructs

Official docs is [here](http://sphinx-doc.org/markup/index.html).

#### toctree

```python toctree
.. toctree::
   :maxdepth: 2

.. toctree::
   :numbered:

.. toctree::
   :caption: Table of Contents
   :name: mastertoc

.. toctree::
   :titlesonly:

.. toctree::
   :glob:

.. toctree::
   :hidden:

.. toctree::
   :includehidden:
```

#### paragraph-level makeup

```python paragraph-level
.. note::
.. warning::

.. versionadded:: 2.5
.. versionchanged:: version
.. deprecated:: 3.1

.. seealso::

.. rubric:: title

.. centered:: LICENSE AGREEMENT

.. hlist::
   :columns: 3

   * A list of
   * short items
   * that should be
   * displayed
   * horizontally

.. glossary::
.. productionlist::
```

#### codes

Official docs is [here](http://sphinx-doc.org/markup/code.html).

```python codes
.. highlight:: c
.. code-block:: ruby

.. highlight:: python
   :linenothreshold: 5

.. code-block:: ruby
   :linenos:

.. code-block:: python
   :emphasize-lines: 3,5

.. literalinclude:: example.py

.. literalinclude:: example.rb
   :language: ruby
   :emphasize-lines: 12,15-18
   :linenos:

.. literalinclude:: example.py
   :encoding: latin-1

.. literalinclude:: example.py
   :pyobject: Timer.start

.. literalinclude:: example.py
   :lines: 1,3,5-10,20-

.. literalinclude:: example.py
   :diff: example.py.orig

.. code-block:: python
   :caption: this.py
   :name: this-py

.. literalinclude:: example.rb
   :language: ruby
   :dedent: 4
   :lines: 10-15
```

#### inline makeup

Official docs is [here](http://sphinx-doc.org/markup/inline.html).

```python inline
:any:
:ref:
:doc:

:download:

:numref:

:envvar:
:token:
:keyword:
:option:
:term:

:abbr:
:command:
:dfn:
:file:
:guilabel:
:kbd:
:mailheader:
:makevar:
:manpage:
:menuselection:
:mimetype:
:newsgroup:
:program:
:regexp:
:samp:
:pep:
:rfc:

|release|
|version|
|today|
```

#### others

```python otheres
:fieldname: Field content

.. sectionauthor:: name <email>
.. sectionauthor:: Guido van Rossum <guido@python.org>
.. codeauthor:: name <email>

.. index:: <entries>
.. index::
   single: execution; context
   module: __main__
   module: sys
   triple: module; search; path

.. index:: Python
.. index:: ! Python
.. index:: BNF, grammar, syntax, notation

.. only:: <expression>
.. only:: html and draft

.. tabularcolumns:: column spec
|l|l|l|
```

### Primer

Official docs is [here](http://sphinx-doc.org/rest.html).



## References

- [Official Docs](http://sphinx-doc.org/contents.html)
- [sphinxcontrib-httpdomain](https://pythonhosted.org/sphinxcontrib-httpdomain/)
