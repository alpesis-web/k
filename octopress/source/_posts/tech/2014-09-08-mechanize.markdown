---
layout: post_tech
title:  "Mechanize"
date:   2014-09-08 21:15:00 +0800
comments: true
categories: [tech]
tags: [python]
---

Mechanize is to simulate a browser.

```python
import mechanize 
import cookielib

from bs4 import BeautifulSoup 

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
```



