---
layout: post_tech
title: "Regular Expression"
date: 2014-07-13 12:46:55 +0800
comments: true
categories: [tech]
tags: [regex, python]
---

## 1. Regex

### 1.1. Elements

symbols: `. ^ $ * + ? { [ ] \ | ( )`


| Type      | Regex | Expression                 | Regex | Expression                 |
|:----------|:------|:---------------------------|:------|:---------------------------|
| container | ()    | all                        |
|           | []    | single selection           |
| match     | ?     | 0/1, {0,1}                 |
|           | +     | >=1, {1,}                  |
|           | *     | >=0, {0,}                  |
|           | .     | any except \n              |
|           | {m,n} | [m,n] times                |
|           | \     | \\\\section = r"\\section" |
| operator  | `|`   | or                         |
| boundary  | ^     | not, start                 | $     | not, end                   |
|           | \A    | match, start               | \Z    | match, end                 |
|           | \b    | match, between             | \B    | not match, between         |

.


### 1.2. Combinations

| Regex | Expression    | Regex | Expression     | Meaning |
|:------|:--------------|:------|:---------------|:--------|
| \d    | [0-9]         | \D    | [^0-9]         | digits  |
| \s    | [ "t"n"r"f"v] | \S    | [^ "t"n"r"f"v] | spaces  |
| \w    | [a-zA-Z0-9]   | \W    | [^a-zA-Z0-9]   | words   |

.

### 1.3. functions

| Function   | Description                                         |
|:-----------|:----------------------------------------------------|
| match()    | default `None`, |
| search()   | default `None`, the location of the matched strings |
| findall()  | a list of the matched strings       |
| finditer() ||
| group()    | the matched string |
| start()    | the start location |
| end()      | the end location |
| span()     | tuple: [start location, end location] |
| split()    ||
| sub()      ||
| subn()     ||

## 2. Examples

### 2.1. Any

- any except `\n`: `.*`
- any: `[\s\S]*`, `[\d\D]*`, `[\w\W]*`
- all English: `/[ -~]/`
- not greedy: `(.*?)`
- null: `\.*`
- any character (one word/digit/punctuation/space): `[\d\w\u4e00-\u9fa5\s]`


### 2.2. Specific patterns

- select the same pattern: `(\d+)(\w+)$2`

### 2.3. Date and Time

- Year - 1950-2000: `(19[5-9][0-9] | 2000)`
- Month - 1-12: `([0-9]|1[0-2])`
- Date - 0-31: `([0-9]|[1-2][0-9]|3[0-1])`
- Date: 1-20-2000 or 1/20/2000 or 1.20.2000: `$2` will be the same pattern as `([/|-|.])`

```
([0-9]|[1-2][0-9]|3[0-1])([/|-|.])
([0-9]|1[0-2])$2
(19[5-9][0-9] | 2000)
``` 

### 2.4. HTML

```html
<b><p>Hello World</p><p>Good Bye</p></b>
```
 
- get "Hello World": `<\w+><(\w+)>(.*?)</$1>`
- `(.*?)`: not greedy, get the first content of `<p>...</p>`
- `<(\w+)>...</$1>`: get the same tab
