---
title: ruby
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-06-13T11:48:45+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---


## [Install with _bundler_](http://bundler.io/)
```
gem install bundler
bundle install
```

## How to install gem with a specified version?
Use the _-v_ flag like this:
`gem install fog -v 1.8`

## How to install gem behind a proxy?
Like this: `gem install -p http://54.67.87.47:8083 gollum`

## How to use other mirroring for gem?
Check out [taobao mirroring](https://ruby.taobao.org/index_en.html). 
```
$ gem sources --remove https://rubygems.org/
$ gem sources -a https://ruby.taobao.org/
$ gem sources -l
```