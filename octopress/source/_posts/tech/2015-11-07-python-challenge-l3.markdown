---
layout: post_tech
title: "Python Challenge L3"
date: 2015-11-07 20:35:25 +0800
comments: true
categories: [tech]
tags: [python, Python Challenge]
toc: false
---

[Python Challenge]()

## L3. Equality

[L3](http://www.pythonchallenge.com/pc/def/equality.html)

One small letter, surrounded by EXACTLY three big bodyguards on each of its
sides.

```python equality.py
import re

def search_equality(text):
    """ Return the letter if that matches the pattern, else return None
    """

    # pattern: [1 lower](3 uppers)[1 lower](3 uppers)[1 lower], e.g.: aBBDiLDMx
    pattern = "[a-z][A-Z][A-Z][A-Z][a-z][A-Z][A-Z][A-Z][a-z]"
    equality = re.search(pattern, text)
    if equality:
        # just return the lowercase on the position 5 (index[4])
        return equality.group()[4]
    else:
        return None

if __name__ == '__main__':

    text_path = "./data/3_equality.txt"

    equalities = []
    text = open(text_path, 'r')
    for line in text.readlines():
        # search the letters those matching the patterns
        equality = search_equality(line.strip())
        # just append the valid letters, None exclusive
        equalities.append(equality) if equality is not None else equalities
    text.close()

    print ''.join(letter for letter in equalities)
```

output

```python equality ouptut
linkedlist
```

Update the url with `linkedlist`, then jump to a new page with `linkedlist.php`,
update the url with `linkedlist.php`, then jump to L4.
