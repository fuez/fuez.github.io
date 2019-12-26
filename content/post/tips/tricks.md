---
title: tricks
date: 2018-11-21T05:37:43+08:00
lastmod: 2019-12-26T10:12:46Z
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

## How to download research papers?

> First search for the topic in ieee site - http://ieeexplore.ieee.org/Xplor....
> Search the required paper then copy its url or DOI no . Open this site https://sci-hub.tw/ and paste url or DOI no there, the concerned research paper’s pdf will be generated.

## Clever tricks

## [How to use .netrc file to save curl user name and password pair?](https://ec.haxx.se/usingcurl-netrc.html)

Simply create a *.netrc* file in the home dir, and add lines like the following, and then you can just use *-n* option to use credential when calling *curl*

```ini
machine gerrit.umarkcloud.cn
login leowa
password a1KOzMq
```

## How to add inverse matching in regex?

Use *?!* notation, like **^abc(?!x).+$** to exclude strings start with *abcx*.

## How to exclude some packages from vendor.json?

```sh
# use "select" to filter, and "contains" to match path, and finaly piped to "not" to inverse
cat  vendor.json  | jq -c '.package[] | select(.path | contains("sdk-go") | not)'
```

## How to refresh chrome dns?

```
chrome://net-internals/#dns
```
