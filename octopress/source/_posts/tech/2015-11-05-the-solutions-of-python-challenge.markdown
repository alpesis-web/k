---
layout: post_tech
title: "The Solutions of Python Challenge"
date: 2015-11-05 16:07:22 +0800
comments: true
categories: [tech]
tags: [python]
toc: true
---

[Python Challenge](http://www.pythonchallenge.com)

## L0

[L0](http://www.pythonchallenge.com/pc/def/0.html)

Hint: try to change the URL address.

```python
>>> 2**38
274877906944
```


Update the url.

## L1. Map

[L1](http://www.pythonchallenge.com/pc/def/map.html)

everybody thinks twice before solving this.

solution

```python
from string import maketrans

alphabet = "abcdefghijklmnopqrstuvwxyz"
alphabet_decode = "cdefghijklmnopqrstuvwxyzab"
map_table = maketrans(alphabet, alphabet_decode)


if __name__ == '__main__':

    strings = """
    g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc spj.
    """

    url = "map"

    print strings.translate(map_table)
    print url.translate(map_table)
```

result

```
    i hope you didnt translate it by hand. thats what computers are for. doing it in by hand is inefficient and that's why this text is so long. using string.maketrans() is recommended. now apply on the url.

ocr
```

Then update the url with `ocr`.

## L2. OCR

[L2](http://www.pythonchallenge.com/pc/def/ocr.html)

recognize the characters. maybe they are in the book, 
but MAYBE they are in the page source.

```python

```
