---
layout: post_tech
title: "Downloading Images by Batch"
date: 2014-07-12 20:35:22 +0800
comments: true
categories: [tech]
tags: [python, crawler]
---

I would like to download some images from a painting website in batch, but didn’t prefer click the urls of these pictures one by one, then I wrote this program to download them in batch.

- website: [Chinese Painting by Shekwing Tam](http://kankanwoo.com/arts/paintings.asp?c=2)
- libraries: re, urllib2, BeautifulSoup
- features
  - extracting the urls of the pictures from the page
  - downloading these pictures and saving them to the local drive


## 1. Get image urls from the website

```python
import re
import urllib2
from bs4 import BeautifulSoup

def getURLs(url):

    fileURLs = []

    contents = urllib2.urlopen(url).read()
    soup = BeautifulSoup(contents)

    for link in soup.find_all(src=re.compile("/arts/paintings/images/mw")):
        fileURLs.append(link.get('src'))

    return fileURLs
```

## 2. Download the images and save to a path

```python
def download(url, outPath, fileName):
    """ downloading files from the web """

    loadedSize = 0
    bufferSize = 8192

    # getting the url of a file
    rawFile = urllib2.urlopen(url)

    # getting the total size of the file
    meta = rawFile.info()
    fileSize = int(meta.getheaders("Content-Length")[0])

    print "File name: %s, Bytes: %s" % (fileName, fileSize)
    print "Downloading..."
    
    f = open(outPath + fileName, 'wb')
    while True:

        # downloading files by blocks
        buffer = rawFile.read(bufferSize)
        if not buffer:
            break
        f.write(buffer)

        # showing the downloading process
        loadedSize += len(buffer)
        status = r"%10d [%3.2f%%]" % (loadedSize, loadedSize * 100. / fileSize)
        print status
    f.close()

    print "%s: Ready!" % (fileName)
```

## Source Codes

```python
import re
import urllib2
from bs4 import BeautifulSoup

def getURLs(url):

    fileURLs = []

    contents = urllib2.urlopen(url).read()
    soup = BeautifulSoup(contents)

    for link in soup.find_all(src=re.compile("/arts/paintings/images/mw")):
        fileURLs.append(link.get('src'))

    return fileURLs


def download(url, outPath, fileName):
    """ downloading files from the web """

    loadedSize = 0
    bufferSize = 8192

    # getting the url of a file
    rawFile = urllib2.urlopen(url)

    # getting the total size of the file
    meta = rawFile.info()
    fileSize = int(meta.getheaders("Content-Length")[0])

    print "File name: %s, Bytes: %s" % (fileName, fileSize)
    print "Downloading..."
    
    f = open(outPath + fileName, 'wb')
    while True:

        # downloading files by blocks
        buffer = rawFile.read(bufferSize)
        if not buffer:
            break
        f.write(buffer)

        # showing the downloading process
        loadedSize += len(buffer)
        status = r"%10d [%3.2f%%]" % (loadedSize, loadedSize * 100. / fileSize)
        print status
    f.close()

    print "%s: Ready!" % (fileName)



def downloadFiles(fileURLs, outPath):

    for url in fileURLs:
        fileName = url.split('/')[-1]
        url = "http://kankanwoo.com/" + url
        download(url, outPath, fileName)


def main():

    url = "http://kankanwoo.com/arts/paintings.asp?c=2"
    outPath = "/outputs/img/mountains_and_water/"

    fileURLs = getURLs(url)
    downloadFiles(fileURLs, outPath)


if __name__ == "__main__":
    main()
```
