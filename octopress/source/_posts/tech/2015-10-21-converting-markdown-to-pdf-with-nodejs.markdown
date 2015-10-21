---
layout: post_tech
title: "Converting Markdown to PDF with NodeJS"
date: 2015-10-21 19:57:50 +0800
comments: true
categories: [tech]
tags: [nodejs, markdown]
toc: false
---

## Requirements

- OS: OS X
- Language: Node.js/npm
- Dependency: markdown-pdf

## Example

install dependency

```bash
$ npm install markdown-pdf
```

convert markdown to pdf

```javascript md2pdf.js
var md_file = "./file_name.md"
var output_file = "./outputs/file_name.pdf"

var markdownpdf = require("markdown-pdf")
var fs = require("fs")

fs.createReadStream(md_file)
  .pipe(markdownpdf())
  .pipe(fs.createWriteStream(output_file))
```

testing

```bash
$ node md2pdf.js
```


## Reference

- [markdown-pdf](https://www.npmjs.com/package/markdown-pdf)
