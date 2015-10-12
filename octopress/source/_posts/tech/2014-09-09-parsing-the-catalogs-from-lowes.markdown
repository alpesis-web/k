---
layout: post_tech
title:  "Parsing the Catalogs from Lowes"
date:   2014-09-09 20:59:00 +0800
comments: true
categories: [tech]
tags: [python, crawler]
---

## 1. Introduction

I needed to get the catalogs from [Lowe's](http://www.lowes.com) which was
including the department and catagories.

- category #
- categories
- item #
- model #
- product #
- product name


## 2. Steps

- step1. analyze the site struture
- step2. parse all urls of department/categories those containing product info
- step3. parse the product info from the urls one by one
- step4. clean the data with standard formats
- step5. load the data into database


## 3. Tools

- language: Python
- libraries: re, beautifulsoup, mechanize

## 4. Notes

- If crawling data is denied by a site, simulating a browser to click the urls.
- Parsing the data from a same container (div).
- URL/href contains a lot of information, especially the data of product ID,
  dept ID, category ID etc.
- Record all the exceptions, and then re-check.
- It will cost more time if writing the data frequently, but it can record the
  process well if writing the data by a small batch.


## 5. Source Codes

- step2. parse all urls of department/categories those containing product info
- step3. parse the product info from the urls one by one
- step4. clean the data with standard formats

### 5.1. parse all urls of department/categories those containing product info


```python
import os
import sys
reload(sys)
sys.setdefaultencoding( "utf-8" )

import re
import time
import urllib
import urllib2
from bs4 import BeautifulSoup

import pandas

def getHTML(url):

    #time.sleep(1.00)
    
    headers = {'User-Agent' : "Mozilla/5.0 (Windows NT 6.0; WOW64) AppleWebKit/534.24 (KHTML, like Gecko) Chrome/11.0.696.16 Safari/534.24"}
    req = urllib2.Request(url,None,headers)
    html = urllib2.urlopen(req).read()
    urllib2.urlopen(url).close()

    soup = BeautifulSoup(html)

    return soup

def extractAttribute(contents, pattern):
    return re.findall(re.compile(pattern), str(contents))

def extractURL(soup):

    results = soup.find('ul', attrs={'class': 'categories sep'})
    #print results

    # links
    pattern = r"""<a href="(.*) title=.*>"""
    links = extractAttribute(results, pattern)
    #print links

    return links


def extract(url):

    soup = getHTML(url)
    data = extractURL(soup)

    return data

def dfs(links, baseurl, fileName):
    
    for link in links:
        thisLink = link.replace('"', '')
        thisURL = baseurl + thisLink
        newURLs = extract(thisURL)
        if newURLs:
            dfs(newURLs, baseurl, fileName)
        else:
            outCSV(fileName, thisLink)
    

def outCSV(fileName, url):

    with open(fileName, 'a') as f:
        f.write("%s\n" % url)

def main():

    baseurl = "http://www.lowes.com"
    url = "http://www.lowes.com/Hidden-Catalogs/_/N-1z138z4/pl#!"
    outPath = "G:/vimFiles/freelance/20140903-eCatalog/src/outputs/"
    fullurls = "lowes-catalogs-fullurls.csv"

    if os.path.exists(outPath+fullurls):
        os.remove(outPath+cateName)

    links = extract(url)
    dfs(links, baseurl, outPath+fullurls)

    #with open(outPath+fullurls, 'wb') as f:
    #    for line in data:
    #        f.write("%s\n" % line)
    #f.close()


if __name__ == "__main__":
    main()
```


### 5.2. parse the product info from the urls one by one


```python
import os
import sys
reload(sys)
sys.setdefaultencoding( "utf-8" )

import mechanize 
import cookielib 

import re
import time
import urllib
import urllib2
from bs4 import BeautifulSoup

import pandas

def openBrowser():

    # Browser 
    br = mechanize.Browser() 
    # Cookie Jar 
    cj = cookielib.LWPCookieJar() 
    br.set_cookiejar(cj) 

    # Browser options 
    br.set_handle_equiv(True) 
    #br.set_handle_gzip(True) 
    br.set_handle_redirect(True) 
    br.set_handle_referer(True) 
    br.set_handle_robots(False) 

    # Follows refresh 0 but not hangs on refresh > 0 
    br.set_handle_refresh(mechanize._http.HTTPRefreshProcessor(), max_time=1) 
    br.addheaders = [('User-agent', 'Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.9.0.1) Gecko/2008071615 Fedora/3.0.1-1.fc9 Firefox/3.0.1')]

    return br

def getSoup(br, url):

    # Open url
    r = br.open(url) 
    html = r.read() 

    soup = BeautifulSoup(html)
    return soup

def loadURLs(dataFile):
    urls = []
    with open(dataFile, 'rb') as f:
        for line in f.readlines():
            urls.append(line.strip())
    return urls

def getPages(soup):

    results = soup.find('span', attrs={'class': 'totalPages'})
    if results:
        pages = int(results.get_text())
    else:
        pages = 0 

    return pages

def filterRE(results, pattern):
    return re.findall(re.compile(pattern), str(results))

def getProducts(content):
    products = []
    links = []

    results = content.find_all('h3', attrs={'class': 'productTitle'})
    #print results
    pattern1 = r"<a href=.*>([\d\w\u4e00-\u9fa5\s].*)</a>"
    results1 = filterRE(results, pattern1)
    for result in results1:
        products.append(result.strip())
    
    #print products

    pattern2 = r'<a href="(.*)" name=.*>'
    results2 = filterRE(results, pattern2)
    for result in results2:
        links.append(result.strip())

    #print links

    return products, links

def main():

    baseurl = "http://www.lowes.com"
    outPath = "G:/vimFiles/freelance/20140903-eCatalog/src/outputs/"

    br = openBrowser()
    for i in range(21):
        fileName = "lowes-catalogs-fullurls-%s.csv" % str(i)
        urls = loadURLs(outPath+fileName)

        outCSV = outPath+"products-"+fileName
        if os.path.exists(outCSV):
            os.remove(outCSV)
        
        for url in urls:
            thisURL = baseurl + url
            soup = getSoup(br, thisURL)
            pages = getPages(soup)
            #print soup
            #print pages

            if pages == 0:
                with open(outPath+"error-no-products.csv", 'a') as nf:
                    nf.write('%s\n' % (thisURL))

            elif pages == 1:

                content = soup.find('ul', attrs={'id': 'productResults'})
                products, links = getProducts(content)

                if len(products) == len(links):

                    data = pandas.DataFrame({'product': products, 'prodURL': links})
                    data['deptURL'] = thisURL

                    with open(outCSV, 'a') as f:
                        data.to_csv(f, header=False)
                else:
                    with open(outPath+"error-one-page.csv", 'a') as tf:
                        tf.write("%s\n" % thisURL)

            else:
                
                for page in range(pages):
                    pageURL = thisURL + "?&rpp=32&page=" + str(page+1)
                    thisSoup = getSoup(br, pageURL)
                    content = thisSoup.find('ul', attrs={'id': 'productResults'})
                    products, links = getProducts(content)
                    #print pageURL

                    if len(products) == len(links):

                        data = pandas.DataFrame({'product': products, 'prodURL': links})
                        data['deptURL'] = pageURL

                        with open(outCSV, 'a') as f:
                             data.to_csv(f, header=False)

                    else:
                        with open(outPath+"error-more-pages.csv", 'a') as pf:
                            pf.write('%s\n' % (pageURL))



if __name__ == '__main__':
    main()
```



### 5.3. clean the data with standard formats


```python
import re
import pandas as pd

def loadContent(dataFile):

    with open(dataFile, 'rb') as f:
        content = f.read()
    f.close()

    return content

def filterRE(content, pattern):
    return re.findall(re.compile(pattern), str(content))

def extractAttrs(content):

    names = []
    #ids = []
    itemIDs = []
    cateIDs = []
    modelIDs = []
    prodIDs = []
    depts = []


    # names
    pattern = r".*,(.*),http://www.lowes.com/.*"
    results = filterRE(content, pattern)
    for result in results:
        names.append(result)

    # ids 
    #pattern = r"/pd_(.*)_.*__\??"
    #results = filterRE(content, pattern)
    #for result in results:
    #    ids.append(result)

    # itemIDs
    pattern = r"/pd_(\d+)-?"
    results = filterRE(content, pattern)
    for result in results:
        itemIDs.append(result)    

    # cateIDs
    pattern = r"/pd_\d+-(\d+)-?"
    results = filterRE(content, pattern)
    for result in results:
        cateIDs.append(result)

    # modelIDs
    pattern = r"/pd_\d+-\d+-(.*)_.*__\??"
    results = filterRE(content, pattern)
    for result in results:
        modelIDs.append(result.replace('+', ' '))

    # prodIDs
    #pattern = r"productId=(\d+)&amp;"
    pattern = r"productId=(\d+)"
    results = filterRE(content, pattern)
    for result in results:
        prodIDs.append(result)

    # depts
    pattern = r"http://www.lowes.com/(.*)/_/N-.*"
    results = filterRE(content, pattern)
    for result in results:
        depts.append(result.replace('-', ' '))

    cates = []
    for dept in depts:
        cates.append(dept.split('/'))

    #print len(names)
    #print len(itemIDs)
    #print len(cateIDs)
    #print len(modelIDs)
    #print len(prodIDs)
    #print len(cates)

    attrs = pd.DataFrame({'prodName': names,
                          'itemID': itemIDs,
                          'cateID': cateIDs,
                          'modelID': modelIDs,
                          'prodID': prodIDs,
                          'cates': cates})

    return attrs


def main():

    dataPath = "G:/vimFiles/freelance/20140903-eCatalog/data/lowes/products/"
    outPath = "G:/vimFiles/freelance/20140903-eCatalog/src/outputs/"

    #for i in range(21):
    #    dataFile = "products-lowes-catalogs-fullurls-%s.csv" % str(i)

    #    content = loadContent(dataPath+dataFile)
    #    attrs = extractAttrs(content)
    #    attrs.to_csv(outPath+"clean-"+dataFile, header=True, index=False)

    #dataFile = "products-error-one-page.csv"
    #dataFile = "products-error-more-pages.csv"
    dataFile = "products-error-no-products.csv"

    content = loadContent(dataPath+dataFile)
    attrs = extractAttrs(content)
    attrs.to_csv(outPath+"clean-"+dataFile, header=True, index=False)



if __name__ == '__main__':
    main()
```

