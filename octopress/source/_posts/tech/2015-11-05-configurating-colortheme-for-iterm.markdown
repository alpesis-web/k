---
layout: post_tech
title: "Configurating Colortheme for iTerm"
date: 2015-11-05 23:08:34 +0800
comments: true
categories: [tech]
tags: [ide, iterm, vim]
toc: true
---

The colortheme files of iTerm is `*.itermcolors`, everyone can create or update a colortheme and
then import it to iTerm. Alternatively, you can download the colorthemes which are shared in 
public and then update them in iTerm.

If Vim is set as the editor, it would be better to keep the colortheme as the same as iTerm.

## 1. Colorthemes

```bash
$ git clone git://github.com/altercation/solarized.git
$ git clone https://github.com/mbadolato/iTerm2-Color-Schemes.git
```

## 2. iTerm

- iTerm -> Preferences -> Profiles -> Colors -> Load Presets -> import

import the colorthemes

- if solarized: iterm2-colors-soloarized
- if iTerm2-Color-Schemes: `*.itermcolors` 


## 3. .bash_profile

```bash
$ brew install coreutils
$ vim ~/.bash_profile             # add export clicolor
$ source ~/.bash_profile
```

.bash_profile

```bash
export CLICOLOR=1
```

Preview

<img src="https://s-media-cache-ak0.pinimg.com/736x/d7/ba/d3/d7bad3e5432d03d2330b88f49dc4b488.jpg" />

## 4. vim

```bash
# copy the colorscheme
$ mkdir ~/.vim/colors
$ cd ~/.vim/colors
$ cp /path/to/solarized/vim-colors-solarized/colors/solarized.vim ./

# update the .vimrc
$ vim ~/.vimrc
```

.vimrc

```bash
syntax enable
set background=dark
colorscheme solarized
```

Preview

<img src="https://s-media-cache-ak0.pinimg.com/736x/31/f1/93/31f19309e06e9affcd25b81f0b8ddc06.jpg" />
