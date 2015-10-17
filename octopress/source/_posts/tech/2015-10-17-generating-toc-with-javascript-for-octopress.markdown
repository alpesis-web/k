---
layout: post_tech
title: "Generating TOC with javascript for Octopress"
date: 2015-10-17 12:37:04 +0800
comments: true
categories: [tech]
tags: [octopress, javascript, web, ruby]
toc: true
---

Features:

- genearting a table of contents in a article
- user can set toc if true/false


Devkits:

- javascript
- html
- sass


## 1. HTML


### 1.1. toc_generator.html

octopress/source/_includes/toc_generator.html

```html toc_generator.html
	{\% if index %\}
	  {\% comment %\}
	    No table of contents on the index page.
	  {\% endcomment %\}

	{\% elsif page.toc == true %\}
	  <script type="text/javascript">
	  jQuery(document).ready(function() {
	    // Put a TOC right before the entry content.
	    generateTOC('.entry-content', 'Table of Contents');
	  });
	  </script>
	{\% endif %\}
```

### 1.2. article.html

Include `toc_generator.html` at the end of `article.html`

```html article.html
...

{\% include toc_generator.html %\}
```

## 2. Javascript

### 2.1. update the libraries

octopress/source/_includes/head.html

```javascript head.html
<script src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js"></script>
<script src="{\{ root_url }\}/javascripts/jquery.tableofcontents.min.js" type="text/javascript"></script>
<script src="{\{ root_url }\}/javascripts/toc_generator.js" type="text/javascript"></script>
```

### 2.2. jquery.tableofcontents.min.js

Go to [TOC jQuery Plugin](http://fuelyourcoding.com/scripts/toc/), 
download the library, and copy `jquery.tableofcontents.min.js` to `octopress/source/javascript/`.


### 2.3. toc_generator.js

octopress/source/javascript/toc_generator.js

```javascript toc_generator.js
	function generateTOC(insertBefore, heading) {
	  var container = jQuery("<div id='tocBlock'></div>");
	  var div = jQuery("<ul id='toc'></ul>");
	  var content = $(insertBefore).first();

	  if (heading != undefined && heading != null) {
	    container.append('<span class="tocHeading">' + heading + '</span>');
	  }

	  div.tableOfContents(content);
	  container.append(div);
	  container.insertBefore(insertBefore);
	}
```

## 3. SASS

### 3.1. _utilities.scss

octopress/sass/base/_utilities.scss

```css _utilities.scss
	@mixin rounded-border($radius: 10px) {
	    border-radius: $radius;
	    -moz-border-radius: $radius;
	   padding: $radius;
	}

	@mixin centered($width: auto) {
	    width: $width !important;
	    margin-left: auto !important;
	    margin-right: auto !important;
	}

	@mixin drop-shadow-right-bottom($width: 5px, $color: #999) {
	    box-shadow: $width $width $width $color;
	    -moz-box-shadow: $width $width $width $color;
	    -webkit-box-shadow: $width $width $width $color;
	}
```

### 3.2. _toc_generator.scss

ocotpress/sass/partials/_toc_generator.scss

```css _toc_generator.scss
	bg: #e8e8e8;
	$toc-incr: 5px;

	div#tocBlock {
	    @include drop-shadow-right-bottom(5px, #999);
	    @include rounded-border(10px);
	    float: right;
	    font-size: 10pt;
	    width: 300px;
	    padding-left: 20px;
	    padding-right: 10px;
	    padding-top: 10px;
	    padding-bottom: 0px;

	    background: $toc-bg;
	    border: solid 1px #ccc;
	    margin: 0 0 10px 15px;

	    .tocHeading {
		font-weight: bold;
		font-size: 125%;
	    }

	    ul { list-style: none; }

	    #toc {
		background: $toc-bg;
		ul {
		    li {
			margin-left: $toc-incr !important;
			padding: 0 !important;
			list-style: disc;
		    }
		}
	    }
	}
```

### 3.3. _partials.scss

octopress/sass/_particials.scss

```css _particials.scss
    @import "partials/toc_generator";
```

## 4. Rakefile

`octopress/Rakefile`: update the head variable for `new_post_tech`, `new_post_art` and `new_post_life`.

```ruby Rakefile
    post.puts "toc: false"
```

## 5. Testing

`rake new_post_tech['post_name']`, check the header if `toc: false` exists.

If `toc: true`, the table of contents will be generated, if `toc:false`, no table of contents.

## Reference

- [Generating a table of contents in Octopress](http://brizzled.clapper.org/blog/2012/02/04/generating-a-table-of-contents-in-octopress/)
