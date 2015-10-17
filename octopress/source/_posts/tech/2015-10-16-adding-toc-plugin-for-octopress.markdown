---
layout: post_tech
title: "Adding TOC Plugin for Octopress"
date: 2015-10-16 22:31:58 +0800
comments: true
categories: [tech]
tags: [ruby, octopress, jekyll, web] 
---

## 1. Plugin: toc_generator.rb

### 1.1. toc_generator.rb

octopress/plugins/toc_generator.rb

```ruby toc_generator.rb
require 'nokogiri'

module Jekyll

  module TOCGenerator

    TOGGLE_HTML = '<div id="toctitle"><h2>%1</h2>%2</div>'
    TOC_CONTAINER_HTML = '<div id="toc-container"><table class="toc" id="toc"><tbody><tr><td>%1<ul>%2</ul></td></tr></tbody></table></div>'
    HIDE_HTML = '<span class="toctoggle">[<a id="toctogglelink" class="internal" href="#">%1</a>]</span>'

    def toc_generate(html)
      # No Toc can be specified on every single page
      # For example the index page has no table of contents
      no_toc = @context.environments.first["page"]["noToc"] || false;

      return html if no_toc

      config = @context.registers[:site].config

      # Minimum number of items needed to show TOC, default 0 (0 means no minimum)
      min_items_to_show_toc = config["minItemsToShowToc"] || 0

      anchor_prefix = config["anchorPrefix"] || 'tocAnchor-'

      # better for traditional page seo, commonlly use h1 as title
      toc_top_tag = config["tocTopTag"] || 'h1'
      toc_top_tag = toc_top_tag.gsub(/h/, '').to_i

      toc_top_tag = 5 if toc_top_tag > 5

      toc_sec_tag = toc_top_tag + 1
      toc_top_tag = "h#{toc_top_tag}"
      toc_sec_tag = "h#{toc_sec_tag}"


      # Text labels
      contents_label     = config["contentsLabel"] || 'Contents'
      hide_label         = config["hideLabel"] || 'hide'
      # show_label       = config["showLabel"] || 'show' # unused
      show_toggle_button = config["showToggleButton"]

      toc_html = ''
      toc_level = 1
      toc_section = 1
      item_number = 1
      level_html = ''

      doc = Nokogiri::HTML(html)

      # Find H1 tag and all its H2 siblings until next H1
      doc.css(toc_top_tag).each do |tag|
        # TODO This XPATH expression can greatly improved
        ct    = tag.xpath("count(following-sibling::#{toc_top_tag})")
        sects = tag.xpath("following-sibling::#{toc_sec_tag}[count(following-sibling::#{toc_top_tag})=#{ct}]")

        level_html    = '';
        inner_section = 0;

        sects.map.each do |sect|
          inner_section += 1;
          anchor_id = [
                        anchor_prefix, toc_level, '-', toc_section, '-',
                        inner_section
                      ].map(&:to_s).join ''

          sect['id'] = "#{anchor_id}"

          level_html += create_level_html(anchor_id,
                                          toc_level + 1,
                                          toc_section + inner_section,
                                          item_number.to_s + '.' + inner_section.to_s,
                                          sect.text,
                                          '')
        end

        level_html = '<ul>' + level_html + '</ul>' if level_html.length > 0

        anchor_id = anchor_prefix + toc_level.to_s + '-' + toc_section.to_s;
        tag['id'] = "#{anchor_id}"

        toc_html += create_level_html(anchor_id,
                                      toc_level,
                                      toc_section,
                                      item_number,
                                      tag.text,
                                      level_html);

        toc_section += 1 + inner_section;
        item_number += 1;
      end

      # for convenience item_number starts from 1
      # so we decrement it to obtain the index count
      toc_index_count = item_number - 1

      return html unless toc_html.length > 0

      hide_html = '';
      hide_html = HIDE_HTML.gsub('%1', hide_label) if (show_toggle_button)

      if min_items_to_show_toc <= toc_index_count
        replaced_toggle_html = TOGGLE_HTML
        .gsub('%1', contents_label)
        .gsub('%2', hide_html);

        toc_table = TOC_CONTAINER_HTML
        .gsub('%1', replaced_toggle_html)
        .gsub('%2', toc_html);

        doc.css('body').children.before(toc_table)
      end

      doc.css('body').children.to_xhtml(indent:3, indent_text:" ")
    end

    private

    def create_level_html(anchor_id, toc_level, toc_section, tocNumber, tocText, tocInner)
      link = '<a href="#%1"><span class="tocnumber">%2</span> <span class="toctext">%3</span></a>%4'
      .gsub('%1', anchor_id.to_s)
      .gsub('%2', tocNumber.to_s)
      .gsub('%3', tocText)
      .gsub('%4', tocInner ? tocInner : '');
      '<li class="toc_level-%1 toc_section-%2">%3</li>'
      .gsub('%1', toc_level.to_s)
      .gsub('%2', toc_section.to_s)
      .gsub('%3', link)
    end

  end

end

Liquid::Template.register_filter(Jekyll::TOCGenerator)
```

### 1.2. Gemfile

```
gem 'nokogiri'
```


### 1.3. _config.yml

```yml _config.yml
#---------------------------#
#      TOC Settings         #
#---------------------------#

minItemsToShowToc: 0
tocTopTag: h1
anchorPrefix: tocAnchor-
showToggleButton: false
```

Variable definitions

<table>
<tr>
<th>Parameter name</th>
<th>Description</th>
<th>Default value</th>
</tr>

<tr>
<td>minItemsToShowToc</td>
<td>Minimum number of items to show the TOC<br/>Suppose you want to generated the TOC only if there are at least 3 H1</td>
<td>0 (no limit)</td>
</tr>

<tr>
<td>tocTopTag</td>
<td>The top level tag given nokogiri to find<br/>Suppose you want to generated the TOC start form h1 to h5</td>
<td>h1</td>
</tr>
<tr>
<td>anchorPrefix</td>
<td>The prefix used to generate the anchor name</td>
<td>tocAnchor-</td>
</tr>

<tr>
<td>showToggleButton</td>
<td>The TOC has a button used to collapse/expand the list, this requires a little of Javascript
<br/>This package contains a jQuery plugin to handle the click</td>
<td>false</td>
</tr>
</table>

## 2. HTML: source/_includes/article.html

source/_includes/article.html

```html article.html
    <div>
        {\{ content | toc_generate }\}
    </div>
```

## 3. SASS: _toc_generator.scss

sass/plugins/_toc_generator.scss

```css _toc_generator.scss
#toc, .toc, .mw-warning {
    background-color: #F9F9F9;
    border: 1px solid #AAAAAA;
    font-size: 95%;
    padding: 5px;
}
#toc h2, .toc h2 {
    border: medium none;
    display: inline;
    font-size: 100%;
    font-weight: bold;
    padding: 0;
}
#toc #toctitle, .toc #toctitle, #toc .toctitle, .toc .toctitle {
    text-align: center;
    margin:0 8px 4px 14px;
    padding-top:8px;
    line-height: 1.5;
    overflow:hidden;
}


#toc ul, .toc ul {
    list-style-image: none;
    list-style-type: none;
    margin:0 8px 7px 14px;
    padding-left: 0;
    text-align: left;
}
#toc ul ul, .toc ul ul {
    margin: 0 0 0 2em;
}
#toc .toctoggle, .toc .toctoggle {
    font-size: 94%;
}

#toc ul li {
    list-style-type: none;
    padding-left: 0;
}

#toc-container {
	margin-bottom: 10px;
}
```

## 4. Javascript: jquery.tocLight.js

```javascript jquery.tocLight.js
/*
 * jQuery Table of Content Generator Support for Jekyll v1.0
 *
 * https://github.com/dafi/jekyll-tocmd-generator
 * Examples and documentation at: https://github.com/dafi/jekyll-tocmd-generator
 *
 * Requires: jQuery v1.7+
 *
 * Copyright (c) 2013 Davide Ficano
 *
 * Dual licensed under the MIT and GPL licenses:
 *   http://www.opensource.org/licenses/mit-license.php
 *   http://www.gnu.org/licenses/gpl.html
 */
(function($) {
    $.toc = {};
    $.toc.clickHideButton = function(settings) {
        var config = {
            saveShowStatus: false,
            hideText: 'hide',
            showText: 'show'};

        if (settings) {
            $.extend(config, settings);
        }

        $('#toctogglelink').click(function() {
            var ul = $($('#toc ul')[0]);
            
            if (ul.is(':visible')) {
                ul.hide();
                $(this).text(config.showText);
                if (config.saveShowStatus) {
                    $.cookie('toc-hide', '1', { expires: 365, path: '/' });
                }
                $('#toc').addClass('tochidden');
            } else {
                ul.show();
                $(this).text(config.hideText);
                if (config.saveShowStatus) {
                    $.removeCookie('toc-hide', { path: '/' });
                }
                $('#toc').removeClass('tochidden');
            }
            return false;
        });

        if (config.saveShowStatus && $.cookie('toc-hide')) {
            var ul = $($('#toc ul')[0]);
            
            ul.hide();
            $('#toctogglelink').text(config.showText);
            $('#toc').addClass('tochidden');
        }

    }
})(jQuery);
```

update the html template

```html article.html
<!DOCTYPE html>
<html>
  <head>
    <title>{\{ page.title }\}</title>

    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
    <script src="js/jquery.tocLight.js"></script>

    <script type="text/javascript">
        $(function() {
          $.toc.clickHideButton();
        });
  </script>

  </head>
  <body id="small">
        {\{ content | toc_generate }\}
  </body>
</html>
```

## 4. Testing

`rake generate` and `rake preview`

## Reference

- [jekyll-toc-generator](https://github.com/dafi/jekyll-toc-generator)
