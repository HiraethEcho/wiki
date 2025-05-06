---
title: regex
toc: true
tags:
    - handbook
date: 2025-05-05
dg-publish: false
---

# regex

## vim

## lua

## shell

## regex

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [quickref.me](https://quickref.me/regex)

> A quick reference for regular expressions (regex), including symbols, ranges, grouping, assertions an......

#### [#](#re-search)re.search()

```
>>> sentence = 'This is a sample string'
>>> bool(re.search(r'this', sentence, flags=re.I))
True
>>> bool(re.search(r'xyz', sentence))
False


```

#### [#](#re-findall)re.findall()

```
>>> re.findall(r'\bs?pare?\b', 'par spar apparent spare part pare')
['par', 'spar', 'spare', 'pare']
>>> re.findall(r'\b0*[1-9]\d{2,}\b', '0501 035 154 12 26 98234')
['0501', '154', '98234']


```

#### [#](#re-finditer)re.finditer()

```
>>> m_iter = re.finditer(r'[0-9]+', '45 349 651 593 4 204')
>>> [m[0] for m in m_iter if int(m[0]) < 350]
['45', '349', '4', '204']


```

#### [#](#re-split)re.split()

```
>>> re.split(r'\d+', 'Sample123string42with777numbers')
['Sample', 'string', 'with', 'numbers']


```

#### [#](#re-sub)re.sub()

```
>>> ip_lines = "catapults\nconcatenate\ncat"
>>> print(re.sub(r'^', r'* ', ip_lines, flags=re.M))
* catapults
* concatenate
* cat


```

#### [#](#re-compile)re.compile()

```
>>> pet = re.compile(r'dog')
>>> type(pet)
<class '_sre.SRE_Pattern'>
>>> bool(pet.search('They bought a dog'))
True
>>> bool(pet.search('A cat crossed their path'))
False


```

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.hackingnote.com](https://www.hackingnote.com/en/cheatsheets/regex/)

> HackingNote

## Syntax

- `|`: or
- `()`: group

### Characters

- `.`: any character (dot matches everything except newlines)
- `\w`: alphanumeric character plus `_`, equivalent to `[A-Za-z0-9_]`
- `\W`: non-alphanumeric character excluding `_`, equivalent to `[^A-Za-z0-9_]`
- `\s`: whitespace
- `\S`: anything BUT whitespace
- `\d`: digit, equivalent to `[0-9]`
- `\D`: non-digit, equivalent to `[^0-9]`
- `[...]`: one of the characters
- `[^...]`: anything but the characters listed

### Anchors

- `^`: beginning of a line or string
- `$`: end of a line or string
- `\b`: zero-width word-boundary (like the caret and the dollar sign)
- `\A`: Matches the beginning of a string (but not an internal line).
- `\z`: Matches the end of a string (but not an internal line).

### Repetition Operators

- `?`: match 0 or 1 times
- `+`: match at least once
- `*`: match 0 or multiple times
- `{M,N}`: minimum M matches and maximum N matches
    - `{M,}`: match at least M times
    - `{0,N}`: match at most N times

## Greedy vs Lazy

- `.*`: match as long as possible
- `.*?`: match as short as possible

## BRE vs ERE vs PCRE

The only difference between basic and extended regular expressions is in the behavior of a few characters: `?`, `+`, parentheses (`()`), and braces (`{}`).

- **basic regular expressions (BRE)**: should be escaped to behave as special characters
- **extended regular expressions (ERE)** : should be escaped to match a literal character.
- **Perl Compatible Regular Expressions (PCRE)**: much more powerful and flexible than BRE and ERE.

Multiple flavors may be supported by the tools:

- sed
    - `sed`: basic
    - `sed -E`: extended
- grep
    - `grep`: basic
    - `egrep` or `grep -E`

## JavaScript

- `str.search`
- `str.match`
- `str.matchAll`
- `str.replace`

Example: split country name and country code in strings like "China (CN)"

```
> s = "China (CN)";
'China (CN)'
> match = s.match(/\((.*?)\)/)
[ '(CN)', 'CN', index: 6, input: 'China (CN)', groups: undefined ]
> match[1]
'CN'
> s.substring(0, match.index).trim()
'China'


```

Match all:

```
const regex = /.*/g;
const matches = content.matchAll(regex);
for (let match of matches) {


}


```

### Literal vs. Constructor

- Literal: `re = /.../g`
- Constructor: `re = new RegExp("...")`
    - can use string concat: `re = new RegExp("..." + some_variable + "...")`

### Local vs. Global

- `re = /.../`: `re.match(str)` will return a list of captures of the FIRST match.
- `re = /.../g`: `re.match(str)` will return a list of matches but NOT captures.

### match vs. exec

- `str.match()`: as stated above.
- `regex.exec()`: return captures, more detailed info; exec multiple times.

Example

```
var match;
while ((match = re.exec(str)) !== null) {}


```

## Python

`match`, `search` and `findall`:

- `re.match()`: only match at the beginning of the string, returns a `match` object.
- `re.search()`: locate a match anywhere in string, returns a `match` object.
- `re.findall()`: find all occurrences, returns a list of strings.

```
>>> type(re.search("foo", "foobarfoo"))
<class '_sre.SRE_Match'>
>>> type(re.match("foo", "foobarfoo"))
<class '_sre.SRE_Match'>


```

### re.match()/re.search()

`re.match()` and `re.search()` return a `match` object:

```
>>> match = re.search("f(.*?),", "foo,faa,fuu,bar")
>>> match.groups()
('oo',)


```

`match.group(0)` returns the string snippet that matches the pattern:

```
>>> match.group(0)
'foo,'


```

other `group` captures the ones in `()`:

```
>>> match.group(1)
'oo'


```

### re.findall()

`re.findall()` returns a list, extract value using `[]`:

```
>>> match = re.findall("f(.*?),", "foo,faa,fuu,bar")
>>> match
['oo', 'aa', 'uu']
>>> match[0]
'oo'


```

### Compiled Patterns

```
pattern = re.compile(pattern_string)
result = pattern.match(string)


```

is equivalent to

```
result = re.match(pattern_string, string)


```

`re.compile()` returns a `SRE_Pattern` object:

```
>>> type(re.compile("pattern"))
<class '_sre.SRE_Pattern'>


```
