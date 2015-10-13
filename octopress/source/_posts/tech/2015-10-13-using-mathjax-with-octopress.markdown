---
layout: post_tech
title: "Using MathJax with Octopress"
date: 2015-10-13 10:26:29 +0800
comments: true
categories: [tech]
tags: [ruby, octopress, mathjax, javascript] 
---

## 1. Adding MathJax

```bash
$ vim octopress/_includes/mathjax.html
```

mathjax.html

```html
<!-- MathJax -->
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
      }
    });
</script>
<script type="text/x-mathjax-config">
    MathJax.Hub.Queue(function() {
        var all = MathJax.Hub.getAllJax(), i;
        for(i=0; i < all.length; i += 1) {
            all[i].SourceElement().parentNode.className += ' has-jax';
        }
    });
</script>
<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
```

## 2. Including MathJax in head.html

```bash
$ vim octopress/_includes/head.html
```

head.html

```html
{\% include mathjax.html %\}
```


