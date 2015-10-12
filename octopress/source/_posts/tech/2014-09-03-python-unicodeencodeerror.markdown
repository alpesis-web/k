---
layout: post_tech
title: "Python UnicodeEncodeError"
date: 2014-09-03 20:22:00 +0800
comments: true
categories: [tech]
tags: [python]
---

Error message

```python
UnicodeEncodeError: 'ascii' codec can't encode characters in position 32-34: ordinal not in range(128) 
```

Solution

```python
# add these lines at the top

import sys
reload(sys)
sys.setdefaultencoding( "utf-8" )
```
