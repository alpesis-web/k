---
layout: post_tech
title:  "Multiple Github Pages and Custom Domains via DNS"
date:   2014-08-07 17:58:22 +0800
comments: true
categories: [tech]
tags: [web, dns, github]
---

I have multiple projects hosted on Github Pages, but I would like to create the
subdomains for specific projects under one domain, then I set up the CNAME and
DNS like this:

    Project 1: username.github.io/project1
    - CNAME file: project1.domain-name.com
    - DNS CNAME:
        - host: project1
        - points to: username.github.io

    Project 2: username.github.io/project2
    - CNAME file: project2.domain-name.com
    - DNS CNAME:
        - host: project2
        - points to: username.github.io


Then it worked.


## How did I config my domain name?

- step1. add a `CNAME` file under the project folder

        CNAME: domain-name.com

- step2. add a new record of `DNS CNAME`

        DNS CNAME (for subdomain name):
        - host: subdomain-name
        - points to: project link

- step3. updated `url` in the `_config.yml`

        baseurl: domain-name
        url: domain-name


## Reference

[Multiple GitHub Pages and custom domains via DNS][link]

  [link]: https://stackoverflow.com/questions/10685961/multiple-github-pages-and-custom-domains-via-dns
