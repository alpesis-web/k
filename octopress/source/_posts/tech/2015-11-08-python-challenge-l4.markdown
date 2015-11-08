---
layout: post_tech
title: "Python Challenge L4"
date: 2015-11-08 21:30:05 +0800
comments: true
categories: [tech]
tags: [python, Python Challenge]
toc: false
---

[Python Challenge]()

## L4. Linkedlist

[L4](http://www.pythonchallenge.com/pc/def/linkedlist.php)

urllib may help. DON'T TRY ALL NOTHINGS, since it will never end. 400 times is more than enough.

```python linkedlist.py
import urllib

def get_param(param_value):
    """Return the nothing_value from the specific url
    """

    param = urllib.urlencode({'nothing': param_value})
    url = "http://www.pythonchallenge.com/pc/def/linkedlist.php?%s" % param
    content = urllib.urlopen(url).read()
    # split the content by " " and return the last word
    value = content.split(" ")[-1]

    return value


if __name__ == '__main__':

    init_value = "12345"
    param = get_param(init_value)
    while param > 0:
        print param
        param = get_param(param)
```

The figures are repeating, but once `peak.html` occurs, update the url with
`peak.html`, then jump to L5.
