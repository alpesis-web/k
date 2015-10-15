---
layout: post_tech
title: "Octopress Construct"
date: 2015-10-06 04:06:04 +0800
comments: true
categories: [tech]
tags: [ruby, octopress]
---

## 1. Folder Construct

```
./
|---- source/                                   # templates
|---- sass/                                     # scss
|---- ./themes/                                 # themes
|          |---- classic/                       # a theme contains source and sass
|                   |---- source/
|                   |---- sass/
|---- plugins/                                   # ruby plugins
|
|---- Gemfile                                    # gemfile
|---- Rakefile                                   # rakefile
|---- config.rb
|---- config.ru
|---- _config.yml                                # site config file
|
|---- public/                                    # the generated pages
|---- _deploy/                                   # the files for deployment
```

## 2. Static Pages

### 2.1. source

```
source/
  |---- _includes/                  # template partials
  |---- _layouts/                   # page layouts
  |
  |---- page/                       # page construct
  |---- index.html                  # index
  |
  |
  |---- javascripts/
  |---- stylesheets/
  |---- assets/                     # fonts and others
  |---- images/
```

#### 2.1.1. templates

`index.html` and `page/index.html` are the top layers, they are extended from `_layouts` which is contatining the page templates.

All the page layouts `_layouts` are extended from the `_includes` partials.

If you would like to start a new page, follow the steps:

- type `rake new_page['page_name']`
- go to `page_name`
- `vim index.html`
- update the layouts and head_vars
- update the contents, for example, include a partial from `_includes` or rewrite the content corresponding to the requirements


#### 2.1.2. static files

Put the images in the `images`, put the fonts and other files in `assets`, put the javascripts in `javascripts`.

NOTE: For the stylesheets, you can update the styles in `sass/`.


### 2.2. sass

```
sass/
  |---- screen.scss
            |---- custom/
            |---- _base.scss                               # the global settings
            |         |---- _utilities.scss                # mixins
            |         |---- _solarized.scss                # codes
            |         |---- _theme.scss                    # theme
            |         |---- _typography.scss               # DOM elements
            |         |---- _layout.scss                   # layouts
            |
            |---- _partials.scss                           # the partial settings
            |         |---- _header.scss
            |         |---- _navigation.scss
            |         |---- _blog.scss
            |         |---- _sharing.scss                  # sharing
            |         |---- _syntax.scss                   # codes
            |         |---- _archives.scss
            |         |---- _sidebar.scss
            |         |---- _footer.scss
            |
            |---- plugins/                                 # plugin styles
```



### 2.3. .themes

The theme related files are `source` and `sass`.

If install a theme, the step is just copy the `source` and `sass` to `octopress/`, and then replace the old files.

If create a theme, vise versa, just copy `octopress/source` and `octopress/sass` to `.themes/theme_name`.


#### 2.3.1. Installing a theme


```bash
$ cd octopress/.themes
$ git clone https://a_new_theme_url

$ cd octopress/
$ rake install['theme_name']
$ rake generate
```


#### 2.3.2. Creating a theme

```bash
$ cd octopress
$ mkdir .themes/theme_name
$ cp -R source .themes/theme_name/source
$ cp -$ sass .themes/theme_name/sass
```

## 3. Plugins

You can extend various of features by ruby plugins.

If you perfer creating a plugin by yourself, follow the steps:

- `cd plugins`
- `vim plugin_name.rb`
- write the features
- update `Gemfile` if some packages required
- go to `source/_includes/plugins`, write the corresponding template
- go to `source/`, include the plugin in the pages
- `vim _config.yml`, write the config variables if needed
- go to `sass/plugins`, write the coresponding style`
- `rake generate`
- `rake preview`

## 4. Configuration

### 4.1. _config.yml

Since Octopress is the extension of Jekyll, you can check the Jekyll configuration documents for the detailed config variables.


## 5. Commands

### 5.1. Gemfile

Update the gems in the `Gemfile`, especially when you update the plugins.

### 5.2. Rakefile

Update the commands in the `Rakefile`.

#### 5.2.1. Default commands

```bash
$ rake generate
$ rake watch
$ rake preview

$ rake new_post['post_name']
$ rake new_page['page_name']
$ rake isolate

$ rake integrate
$ rake update_sytle
$ rake update_source

$ rake deploy
$ rake gen_deploy
$ rake rsync
$ rake set_root_dir
$ rake setup_github_pages
```

## Reference

- [Octopress Docs](http://octopress.org/docs/)
