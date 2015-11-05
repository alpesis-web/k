---
layout: post_tech
title: "Python Challenge L0-L2"
date: 2015-11-05 16:07:22 +0800
comments: true
categories: [tech]
tags: [python, Python Challenge]
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

```python map.py
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

```python map outputs
    i hope you didnt translate it by hand. thats what computers are for. doing it in by hand is inefficient and that's why this text is so long. using string.maketrans() is recommended. now apply on the url.

ocr
```

Then update the url with `ocr`.

## L2. OCR

[L2](http://www.pythonchallenge.com/pc/def/ocr.html)

recognize the characters. maybe they are in the book, 
but MAYBE they are in the page source.

```python ocr.py
# dictionary: http://www.math.sjsu.edu/~foster/dictionary.txt
# download the dictionary for mapping the words

def print_sorted_keys(dictionary):
    print sorted(dictionary.items(), key=lambda dictionary: dictionary[0])

def print_sorted_values(dictionary):
    print sorted(dictionary.items(), key=lambda dictionary: dictionary[1])

def generate_word_dict(file_path):

    word_counts = {}

    text = open(file_path, 'r')
    for line in text.readlines():
        for char in line:
            if char not in word_counts.keys():
                word_counts[char] = 1
            else:
                word_counts[char] += 1
    text.close()

    return word_counts

def mapping(letters, dictbook):

    words = []

    dictbook = open(dictbook, 'r')
    for line in dictbook.readlines():
        # if set(line.strip()) == set(letters):
        #     words.append(line.strip())
        words.append(line.strip()) if set(line.strip()) == set(letters) else words
    dictbook.close()

    return words

if __name__ == '__main__':

    file_path = './data/2_ocr.txt'
    dictbook = "./data/dictionary.txt"

    # find the letters
    word_counts = generate_word_dict(file_path)
    print_sorted_keys(word_counts)
    print_sorted_values(word_counts)

    # look for a word from a dictionary
    letters = "aeilquty"
    words = mapping(letters, dictbook)
    print words
```

result

```python ocr outputs
[('a', 1), ('e', 1), ('i', 1), ('l', 1), ('q', 1), ('u', 1), ('t', 1), ('y', 1), ('\n', 1220), ('^', 6030), ('*', 6034), ('&', 6043), ('$', 6046), ('{', 6046), ('+', 6066), ('!', 6079), ('%', 6104), ('}', 6105), ('[', 6108), ('_', 6112), ('#', 6115), (']', 6152), ('(', 6154), ('@', 6157), (')', 6186)]
['equality']
```

Update the url with `equality`.
