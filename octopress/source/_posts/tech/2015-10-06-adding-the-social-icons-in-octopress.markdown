---
layout: post_tech
title: "Adding the Social Icons in Octopress"
date: 2015-10-06 05:04:17 +0800
comments: true
categories: [tech]
tags: [ruby, octopress, web, html, css]
---

## 1. Perfections: design the icons

Go to [Perfections](http://perfecticons.com), choose the icons you need, select the styles, and then generate the codes.

Once done, download the `fonts.zip`.

## 2. Octopress: integrate the icons

- copy the `fonts.zip` to `octopress/source/assets/fonts/`
- copy the html file to `octopress/source/_includes/social.html`, and then modify the variables
- add the `social.html` to a page
- copy the css file to `octopress/sass/partials/_socials.scss`
- update the links in `octopress/_config.yml`

Once done, run `rake generate` and `rake preview` to check.

